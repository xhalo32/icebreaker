# Ice Breaker

Ice Breaker is a web application designed to allow students to ask
real-time, anonymous questions during class. Once started, it allows the
creation of *rooms*, each with its own set of keys: a private
*instructor key*, and a public *student key*. Each key exposes a
different view of the room; students see only a form that allows them to
submit questions (optionally including a name), whereas instructors see
a stream of incoming questions from students.

The envisioned use of this application is that one of the instructors
(most likely a TA) monitors the instructor view during class, and asks
any incoming questions by proxy. The hope is that this will lower the
barrier for shyer students to ask questions during class.

Ice Breaker was designed to augment, not replace, systems like <a
href="https://piazza.com">Piazza</a>. Because of this, all Ice Breaker
rooms are ephemeral: if the server is restarted, all questions and rooms
are erased. To minimize the overhead of using Ice Breaker, there are
also no users, no sessions, and no administration interface. Rooms are
created by the first person to open them, and keys are given directly in
the room URLs.

## Usage

Once Ice Breaker has been deployed (see below), usage is
straightforward. To create a new room, simply point your browser at
`$URL/room/$ROOM/$KEY`. A student key (`$SKEY`) will be automatically
generated from the instructor key, and will be shown in the instructor
view. Student can now access the room using `$URL/room/$ROOM/$SKEY`.
Other instructors can join the room by using the same URL as was used to
create the room. The rooms should also be easily accessible on mobile
phones.

Instructors can see all questions, whereas students can see none.
Questions/notes posted by other instructors will be highlighted in the
question feed, allowing basic communication between instructors. The
instructor feed will automatically show new questions as they come in on
most modern browsers.

**Note**: Since Ice Breaker deployments are not stateful, a server
restart will wipe all rooms, including their keys. To prevent students
from accidentally re-creating an old room with the student key as the
instructor key when accessing an old URL, instructor keys cannot be on
the form of student keys (eight character hex strings).

## Deployment

Ice Breaker is trivial to deploy:

```shell
gcloud app deploy
```

The repository also contains all the configuration files required to
deploy directly to Google Cloud app engine.

Beware that Google Cloud app engine will put your server to
sleep
after a certain amount of time if you are on the free tier service. This
process effectively terminates the application and restarts it the next
time one of its URLs are accessed, so once this happens, all room state
will be wiped.
