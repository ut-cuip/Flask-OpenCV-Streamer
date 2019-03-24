# Flask-OpenCV-Streamer
A Python package for easily streaming OpenCV footage, even with authentication

## Installation

Install via PyPi using Pip / PipEnv:

`pip install flask_opencv_streamer`

## Usage

Usage is quite straight forward. After importing, you can create as many streamer objects as you wish. 


## Example Code:

### Without authentication (no login required to view page)


```python
from flask_opencv_streamer.streamer import Streamer
import cv2

port = 3030
require_login = False
streamer = Streamer(port, require_login)

# Open video device 0
video_capture = cv2.VideoCapture(0)

while True:
    _, frame = video_capture.read()

    streamer.update_frame(frame)

    if not streamer.is_streaming:
        streamer.start_streaming()

    cv2.waitKey(30)
```

### With authentication (A password will be generated for you, expiring every 24 hrs)

```python
from flask_opencv_streamer.streamer import Streamer
import cv2

port = 3030
require_login = True
login_file = "logins.txt"
login_key = "loginkey.txt
streamer = Streamer(port, require_login, login_file=login_file, login_key=login_key)

# Open video device 0
video_capture = cv2.VideoCapture(0)

while True:
    _, frame = video_capture.read()

    streamer.update_frame(frame)

    if not streamer.is_streaming:
        streamer.start_streaming()

    cv2.waitKey(30)
```

**If there is no logins file or key found at the path given, it will create one for you**. Logins will be stored in a `.txt` file `logins.txt` but will be **encrypted**. Therefore, unless someone has the key (in this example, `loginkey.txt`) the `logins.txt` file will be able to show logins or passwords. It is very unsafe to keep the login key somewhere publicly accessible; it's suggested you hide it well and do not upload it anywhere.

### Adding or removing your own logins

In your **root project directory**, you can include a `templates` folder which can be used for password change templates. Your file names must be:

- `form.html`: Contains the HTML form for the password change.
- `pass.html`: Contains the HTML for a pass event
- `fail.html`: Contains the HTML for a fail event

Samples are included here in this repository
