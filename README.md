# What is this?

<5min setup for telegram notifications about errors, progress, and task-completion.

# How do I use this?

1. `pip install telepy_notify`

2. 30 sec:
  - DM https://t.me/botfather
  - send `/newbot` to ^
  - copy the token out of the response message

3. DM your bot to initialize it (important!)

4. profit

```python
from telepy_notify import Notifier

# 
# basic setup
# 
# NOTE: no errors will be raised, even if someone failed to setup their bot
#       (only warnings will be printed if a token doesnt work)
notify = Notifier(
    # read from file
    token_path="some/file/thats/git/ignored/telegram.token",
    # OR read from ENV var:
    token_env_var="TELEGRAM_TOKEN",
    # OR give the token directly
    token="alkdsfjakjfoirj029294ijfoi24j4-2",
)

# 
# Basic notification
# 
notify.send("Howdy!")
notify.send("Howdy! <b>I'm bold</b>")
notify.send("Howdy! <b>I'm bold</b>\n<code>thing = [1,3,4]</code>")

# 
# get duration and/or error information
# 
with notify.when_done:
    blah_blah_blah = 10
    for blah in range(blah_blah_blah):
        sleep(100)
    raise Exception(f'''Hello Telegram :)''')

# 
# Progress notifications
# 
#    - gives ETA and other time-info
#    - can limit print-rate (by time passed or percent-progress)
#    - will only notify on-print
for progress, epoch in notify.progress([5,6,7], seconds_per_print=100):
    index = progress.index
    
    # do stuff
    import random
    accuracy = random.random()
    if accuracy > 0.99:
        notify.send("Gottem 🎉🎉🎉")
    # end do stuff
    
    progress.message = f"recent accuracy: <code>{accuracy}</code>"

```
