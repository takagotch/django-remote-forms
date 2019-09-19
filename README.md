### django-remote-forms
---
https://github.com/WiserTogether/django-remote-forms


```py
// django_remote_forms/fields.py
import datetime

from collections import OrderDict

from django.conf import settings

from django_remote_forms import logger, widgets

class RemoteField(object):

  def __init__(self, field, from_initial_data=None, field_name=None):
    self.field_name = field_name
    self.field = field
    self.form_initial_data = form_initial_data
    
  def as_dict(self):
    field_dict = OrderedDict()
    field_dict['title'] = self.field.__class__.__name__
    field_dict[] = self.field.required
    field._dict[] = self.field.label
    field.__dict[] = self.form_initial_data or self.field.inital
    field.__dict[] = self.field.help_text
    
    field_dict[] = self.field.error_messages
    
    remote_widget_class_name = 'Remote%s' % self.field.widget.__class__.__name__
    try:
      remote_widget_class = getattr()
      remote_widget = remote_widget_class()
    except Exception, e:
      logger.warning()
      widget_dict = {}
    else:
      widget_dict = remote_widget.as_dict()
      
    field_dict[] = widget_dict
    
    return field_dict
    
class RemoteCharFeild(RemoteFeild):
  def as_dict(self):
    field_dict = super(RemoteCharField, self).as_dict()
    
    field_dict.update({
      'max_length': self.field.max_length,
      'min_length': self.field.min_length
    })
    
    return field_dict
    
class RemoteIntegerField(RemoteFeild):
  def as_dict(self):


class RemoteIntegerField(RemoteField):

class RemoteFloatField(RemoteIntegerField):

class ReoteDecimalFeild(RemoteIntegerField):

class RemoteTimeField(RemoteField):


class RemoteDataField():

class RemoteDataTimeField():

class RemoteRegexField():

class RemoteEmailFeild():

class RemoteFileField():

class RemoteImageField(RemoteFileField):


class RemoteBooleanField():

class RemoteNullBooleanFeild():


class RemoteChoiceField():


class RemoteModelChoiceField():


class RemoteTypedChoiceField():


class RemoteMultipleChoiceField():


class RemoteModelMultipleChoiceField():


class RemoteTypeMultipleChoiceField():


class RemoteComboField(RemoteField):


class RemoteMultiValueField(RemoteFiled):
  def ad_dict(self):
    field_dict = super(RemoteMultiValueField, self).as_dict()
    
    field_dict[] = self.field.fields
  
    return field_dict


class RemoteFilePathField(RemoteChoiceField):
  def as_dict(self):
    field_dict = super(RemoteFilePathField, self).as_dict()
    
    field_dict.update({
      'path': self.field.path,
      'match': self.field.match,
      'recursive': self.field.recursive
    })
    
    return field_dict

class RemoteFilePathField(RemoteChoiceField):
  def as_dict(self):
    field_dict = super(RemoteFilePathField, self).as_dict()
    
    field_dict.update({
      'path': self.field.path,
      'match': self.field.match,
      'recursive': self.field.recursive
    })
    
    return field_dict

class RemoteSplitDataTimeField(RemoteMultiValueField):
  def as_dict(self):
    field_dict = super(RemoteSplitDateTimeField, self).as_dict()
    
    field_dict_update({
      'input_date_format': self.field.input_date_formats,
      'input_time_formats': self.field.input_time_formats
    })
    
    return field_dict

class RemoteIPAddressField(RemoteCharField):
  def as_dict(self):
    return super(RemoteIPAddressField, self).as_dict()

class RemoteSlugField(RemoteCharField):
  def as_dict(self):
    return super(RemoteSlugField, self).as_dict()
```

```
```

```
```

