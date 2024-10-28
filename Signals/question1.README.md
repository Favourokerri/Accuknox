By default, Django signals are executed **synchronously**. This means that the signal handler functions  block the execution of the program until they complete.

To prove this, we can set up a Django signal that includes a delay within the receiver function. If signals were asynchronous by default, the delay would not block the execution of the code following the signal dispatch. However, if signals are synchronous, this delay will block further code execution until the receiver function completes.

### Code Example to Prove Django Signals are Synchronous

```python
import time
from django.db.models.signals import post_save
from django.dispatch import receiver
from django.contrib.auth.models import User

# Define a signal receiver with a delay
@receiver(post_save, sender=User)
def user_saved_signal(sender, instance, **kwargs):
    print("Signal received, starting delay...")
    time.sleep(5)  # Delay for 5 seconds to simulate a long-running task
    print("Delay finished, signal handler complete.")

# Trigger the signal by saving a User instance
def test_signal_synchrony():
    print("Saving user...")
    user = User.objects.create(username="testuser")
    print("User saved successfully.")
```


### Expected Output

When running `test_signal_synchrony()`, the output should be:

```
Saving user...
Signal received, starting delay...
# 5-second delay here
Delay finished, signal handler complete.
User saved successfully.
```

This demonstrates that Django signals are executed synchronously by default.
