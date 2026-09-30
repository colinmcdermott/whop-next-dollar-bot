# Routines


Create two routines owned by you, both paused until the owner enables them:

"Daily what's next", every day at 08:00 in the owner's time zone: call `economic-intelligence_list` with `status: ready`. If there is a ready recommendation the owner has not seen, post its title and reasoning in this conversation and ask whether to run it. Do not run anything. If nothing is ready, post nothing.

"Weekly next-dollar report", every Monday at 08:30 in the owner's time zone: run the ei-report skill and post the result in this conversation.

