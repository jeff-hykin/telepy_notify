# What is this?

<5min setup for telegram notifications about errors, progress, and task-completion. This is an extremely minimal module, with no pip dependencies.

# How do I use this?

1. `pip install telepy_notify`

2. Take 30sec to get a token:
  - open up a chat with https://t.me/botfather
  - send `/newbot` to ^
  - copy the token out of the response message

3. DM your bot to initialize it (important!)

4. Use it

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
    
    # optional: prefix give all messages
    message_prefix="Lambda Machine: Experiment 19:",
    # optional: use this for debugging
    disable=True,
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
        from time import sleep
        sleep(0.1)
    raise Exception(f'''Hello Telegram :)''')
# """
# process took: 1sec, ended at: Sun Aug 20 11:23:38 2023 however it ended with an error: Exception('Hello Telegram :)')
# """

# 
# Progress notifications
# 
#    - gives ETA and other time-info
#    - can limit print-rate (by time passed or percent-progress)
#        e.g. percent_per_print=50 => message me after 50% progress
#        e.g. seconds_per_print=60*30 => message once every 30min
for progress, epoch in notify.progress(range(100), percent_per_print=50, seconds_per_print=60*30):
    index = progress.index
    
    # do stuff
    import random
    accuracy = random.random()
    if accuracy > 0.99:
        notify.send("Gottem 🎉🎉🎉")
    # end do stuff
    
    progress.message = f"recent accuracy: <code>{accuracy}</code>"

# that^ outputs:
# message1:
#     [=================>.................] 50.00% 
#     |  50/100 
#     | remaining: 0sec 
#     | eta: 11:52:48 
#     | elapsed: 0sec 
#     | recent accuracy: 0.5086382690344122
# message2:
#     Done in 1sec at 11:52:49


```
