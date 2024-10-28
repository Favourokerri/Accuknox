Django signals run in the same thread as the caller. This means that the signal handlers execute in the same thread context as the code that triggers the signal. We can demonstrate this behavior by capturing the current thread's information before and after the signal is triggered.

### Code Example to Prove Django Signals are Synchronous

```python
import time
import threading
from django.db.models.signals import post_save
from django.dispatch import receiver
from django.contrib.auth.models import User

# Define a signal receiver with a delay
@receiver(post_save, sender=User)
def user_saved_signal(sender, instance, **kwargs):
    current_thread = threading.current_thread()
    print(f"Signal received in thread: {current_thread.name}")
    print("Signal received, starting delay...")
    time.sleep(5)  # Delay for 5 seconds to simulate a long-running task
    print("Delay finished, signal handler complete.")

# Trigger the signal by saving a User instance
def test_signal_synchrony():
    current_thread = threading.current_thread()
    print(f"Saving user in thread: {current_thread.name}")
    
    user = User.objects.create(username="testuser")
    print("User saved successfully.")

# To execute the test
test_signal_synchrony()

```


### Expected Output

When running `test_signal_synchrony()`, the output should be:

```
Saving user in thread: MainThread
Signal received in thread: MainThread
Signal received, starting delay...
# 5-second delay here
Delay finished, signal handler complete.
User saved successfully.

```
