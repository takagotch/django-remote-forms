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










  

```

```
```

```
```

