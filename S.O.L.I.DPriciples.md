# Let’s deep dive into SOLID principles. 

SOLID is a short form. It stands for

1. Single Responsibility Principle

2. Open and Closed Principle

3. Lisvok Sub situation Principle

4. Interface Segregation Principle

5. Dependency Inversion Principle


## 3. Lisvok Sub situation Principle

Liskov substuition principle states that child class can be substuition of its parent class.
It aims that the child class can assume the place of its parent class without causing any errors.

```python
from abc import ABC, abstractmethod

class Notification(ABC):
    @abstractmethod
    # Here notify takes msg and email as 2 parameters 
    def notify(self, msg, email):
        pass

class Email(Notification):
    def notify(self, message, email):
        print(f'send{message} to {email})

class SMS(Notification):
    def notify(self, message, phone):
        print(f'Send {message} to {phone})

```


### What is happening here is : 

The notification accepts the email as input not phone number as input so we need to change the signature of the notify method.

```python
class Contact:
    def __init__(self, name, email, phone):
        self.name = name
        self.email = email
        self.phone = phone
    

class NotificationManager:
    def __init__(self, notification, contact):
        self.contact = contact
        self.notification = notification
    
    def send(self, msg):
        if isinstance(self.notification, Email):
            self.notification.notify(message, contact.email)
        elif isinstance(self.notification, SMS):
            self.notification.notify(message, contact.phone)
        else:
            raise Exception('The notification is not supported')

if __name__ == '__main__':
    contact = Contact('John Doe', 'john@test.com', '(408)-888-9999')
    notification_manager = NotificationManager(SMS(), contact)
    notification_manager.send('Hello John')

```


### What is the problem with this code ??

Here the SMS and Email subclasses are not easily substuitable in place of its parent class. So we are in a need to check the instance of the subclass.
If we try to replace parent class with subclass we will be sending unnessary/irrelevant data to phone class and email class.
We will send phone number to email class and email to phone number class.


### What can we do ?? 🤔 

👉 First, redefine the notify() method of the Notification class so that it doesn’t include the email parameter:

👉 second change the Email and SMS class to accept its parameters.

👉 Third the notification manager should take notification object as input and call the notify without knowing the subclass.


Putting it altogether:

```python 
from abc import ABC, abstractmethod


class Notification(ABC):
    @abstractmethod
    def notify(self, message):
        pass


class Email(Notification):
    def __init__(self, email):
        self.email = email

    def notify(self, message):
        print(f'Send "{message}" to {self.email}')


class SMS(Notification):
    def __init__(self, phone):
        self.phone = phone

    def notify(self, message):
        print(f'Send "{message}" to {self.phone}')


class Contact:
    def __init__(self, name, email, phone):
        self.name = name
        self.email = email
        self.phone = phone


class NotificationManager:
    def __init__(self, notification):
        self.notification = notification

    def send(self, message):
        self.notification.notify(message)


if __name__ == '__main__':
    contact = Contact('John Doe', 'john@test.com', '(408)-888-9999')

    sms_notification = SMS(contact.phone)
    email_notification = Email(contact.email)

    notification_manager = NotificationManager(sms_notification)
    notification_manager.send('Hello John')

    notification_manager.notification = email_notification
    notification_manager.send('Hi John')

```




    
