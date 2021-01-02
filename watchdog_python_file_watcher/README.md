# Watchdog Python File Watcher

## [Youtube Tutorial Here]()

## Pip Install
```ps1
pip install watchdog --target .\site-packages
```

## module setup
* in watchdog.py
```py
import sys
import time
sys.path.append(sys.path[0] + "\\site-packages")
from watchdog.events import LoggingEventHandler
from watchdog.observers import Observer
```

## Do something on file changes
```py
class WatchDogEvent(LoggingEventHandler):
    def on_modified(self, event):
        print("file modified")
        restart_server = True   
```

## Start the thread 
```py
if __name__ == "__main__":
    path = sys.argv[1] if len(sys.argv) > 1 else '.'
    observer = Observer()
    observer.schedule(WatchDogEvent(), path, recursive=True)

    observer.start()
    try:
        while True:         
            time.sleep(1)
    except KeyboardInterrupt:
        observer.stop()
    observer.join()
       
```


### Resources

[watchdog](https://pypi.org/project/watchdog/)
