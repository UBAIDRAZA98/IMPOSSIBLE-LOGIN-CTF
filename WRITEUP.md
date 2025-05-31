🧩 Challenge Writeup – "IMPOSSIBLE LOGIN"
This CTF challenge is based on logical flow manipulation, session management, and endpoint discovery.

🔍 Step-by-Step Walkthrough
Starting Point – /login
The root (/) redirects to the /login page.

The login form expects a token, but there's no obvious way to get one from the login page itself.

Discovering Token – /pre-auth
Manually navigating to the /pre-auth endpoint returns a token:

php-template
Copy
Edit
Your pre-auth token: <random_token>
This token is stored server-side with a timestamp.

Logging In
Copy the token from /pre-auth and submit it via the login form at /login.

If the token is valid and not older than 20 seconds, login succeeds and the session is set:

python
Copy
Edit
session['logged_in'] = True
session['visited_race'] = False
Exploring the Dashboard – /dashboard
Once logged in, you're redirected to the dashboard.

However, there’s nothing special here—just a decoy to delay exploration.

Clue Discovery – /about
Although /about is not defined in the source code, the hint in the challenge description or possibly from a real interface (if it existed) suggests the word “race”.

This subtle hint leads us to try the /race endpoint.

Visiting the Hidden Endpoint – /race
This page displays a message:

"You feel like you are on the right track..."

More importantly, it sets a session flag:

python
Copy
Edit
session['visited_race'] = True
Accessing the Flag – /get-flag
Trying to visit /get-flag before visiting /race results in:

"You haven't explored enough."

But after visiting /race, the flag is successfully shown:

html
Copy
Edit
ACM{you_found_it}
⚠️ Important Observations:
The token is time-sensitive (valid only for 20 seconds).

The flag is conditionally displayed based on a session variable visited_race.

The /race endpoint acts as a required checkpoint to unlock the actual flag.

Direct access to /get-flag without visiting /race will not reveal the flag.

🛑 Decoys and Misleads:
The /fake-flag route displays a decoy flag: ACM{this_is_a_trap}.

The /debug route exists but returns a 403 Forbidden, serving as a distraction.

The /robots.txt file includes:

text
Copy
Edit
Disallow: /race
Disallow: /debug
This hints to careful players or crawlers that /race is something interesting to check.

