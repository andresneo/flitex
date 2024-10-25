Introduction:
"I've been working on Bernard's idea of creating a custom flight CSV file, and all the work I've been doing revolves around this. However, I’ve encountered some challenges in making it truly 'custom,' particularly with columns like the route and flight reference, which I can't determine without directly accessing the database. Sid has been helping me tackle this issue."

Database connection and first tests:
"Sid helped me set up the correct connection to the database, which was a major issue because I wasn’t getting the latest data. Once we fixed that, I was able to test a flight inside the notebook using this command:

bash
```
python tfms_dbquery12.py --idents DAL1743 -d 20241022
```

But I'm still facing the ‘Graph optimizer error,’ and the system ended up using an unoptimized route."

Next steps:
"To find out if the problem is with the code or the specific flight, I need to try other flights. But for this, I need access to the latest flights directly from the database, which I'll have to do using pgAdmin with Arnau’s help, as Sid suggested.

Sid also recommended using flights with aircraft type B763, although the flight I tested (DAL1743) used a B739. Additionally, I'm using FlightAware as a tool to gather flight information."

Challenges with the custom CSV:
"As mentioned, the main challenge with Bernard’s idea has been the difficulty in making the CSV file truly custom, especially since critical data like the route and flight reference can only be accessed from the database directly."
