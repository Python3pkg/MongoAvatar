# DjangoMongoAvatar
Mongo Avatar is a django like orm for interacting with MongoDB.

# Installation
You can install django-mongo-avatar through pip
$ pip install django-mongo-avatar

# Settings
In your django settings file include 'mongo_avatar' in your INSTALLED_APPS.
INSTALLD_APPS = [
  ...
  'mongo_avatar'
]

Furthermore you have to define atleast one mongo connection in your settings file.

MONGO_CONNECTIONS = {
    'default': {
        'NAME': 'yourdatabase',
        'USER': 'yourusername',
        'PASSWORD': 'yourpassword',
        'HOST': 'localhost',
        'PORT': '27017',
    },
}

# Usage
MongoModelis a django like Model that refers to a collection in mongodb. In the model class you only need to define fields that have special attributes like unique=True or db_index=True otherwise you donot need to define any fields in the model.

Ex: 
from mongo_avatar.models import MongoModel, MongoField
from datetime import datetime

class Link(MongoModel):
    
    link = MongoField(unique=True)
    referrer = MongoField(default=[])
    priority = MongoField(db_index=True)
    is_active = MongoField(default=True)
    created = MongoField(db_index=True, default=datetime.now())
    modified = MongoField(db_index=True)
    
    class Meta:
        collection = 'some_other_collection'
        sync = True
        
# Syncing Models:
through the "mongosync" management command you can sync the models.

Ex:
$ python manage.py mongosync

# Querying Models
Querying works the same as django

Ex:
from app.models import Link
from datetime import datetime
link = Link.objects.create(link='http://github.com', is_active=True, created=datetime.now())
links = Link.objects.filter(link__contains='github.com', created__lte=datetime.now())
links[0].is_active

Note: This is a development version if you encounter any issues feel free to email me on m.haroon.fsd@gmail.com


