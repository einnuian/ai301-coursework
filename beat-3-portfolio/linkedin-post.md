I claimed my first open-source bug in lasagna, a Python tool for testing AI search-and-answer systems, and reproduced it on the latest code. When a free local AI model grades the answers, one of the key scores silently never gets computed, even though the setup check says everything is fine.

The project's owner accepted my claim the same day, thanking me for a clear plan. That plan only existed because a checker I built flagged my first claim: the project expects a list of files, a test, and commands, not just "I'd like to help." My reproduction found the cause, and a quick experiment showed a change that makes the score work again. The fix itself isn't written yet; my next step is a test that fails today, then the fix and a pull request.

What it taught me about how I work:
🔍 Read the project's own rules before speaking up.
🧪 Trust output over confidence: my first experiment looked like it disproved the bug until I found the mistake was in my own logging.
🛠️ When a tool I wrote and the project disagreed, I changed my tool's rules, not the project's.

https://github.com/dolev31/lasagna/issues/113#issuecomment-7085540848

#OpenSource #Python #AI #LearningInPublic
