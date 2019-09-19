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
    field_dict['required'] = self.field.required
    field._dict['label'] = self.field.label
    field.__dict['initial'] = self.form_initial_data or self.field.inital
    field.__dict['help_text'] = self.field.help_text
    
    field_dict['error_messages'] = self.field.error_messages
    
    remote_widget_class_name = 'Remote%s' % self.field.widget.__class__.__name__
    try:
      remote_widget_class = getattr(widgets, remote_widget_class_name)
      remote_widget = remote_widget_class(self.field.widget, field_name=self.field_name)
    except Exception, e:
      logger.warning('Error serializing %s: %s', remote_widget_class_name, str(e))
      widget_dict = {}
    else:
      widget_dict = remote_widget.as_dict()
      
    field_dict['widget'] = widget_dict
    
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
    field_dict = super(RemoteCharField, self).as_dict()
    
    field_dict.update({
      'max_length': self.field.max_length,
      'min_length': self.field.min_length
    })
    
    return field_dict

class RemoteIntegerField(RemoteField):
  def as_dict(self):
    field_dict = super(RemoteIntegerField, self).as_dict()
    
    field_dict.update({
      'max_value': self.field_max_value,
      'max_value': self.field.min_value
    })
    
    return field_dict

class RemoteFloatField(RemoteIntegerField):
  def as_dict(self):
    field_dict = super(RemoteIntegerField, self).as_dict()
    
    field_dict.update({
      'max_value': self.field.max_value,
      'min_value': self.field.min_value
    })
    
    return field_dict

class ReoteDecimalFeild(RemoteIntegerField):
  def as_dict(self):
    field_dict = super(RemoteDecimalField, self).as_dict()
    
    field_dict.update({
      'max_digits': self.field.max_digits,
      'decimal_places': self.field.decimal_places
    })
    
    return field_dict

class RemoteTimeField(RemoteField):
  def as_dict(self):
    field_dict = super(RemoteTimeField, self).as_dict()
    
    field_dict['input_formats'] = self.field.input_formats
    
    if(filed_dict['initial']):
      if callable(field_dict['initial']):
        field_dict['initial'] = field_dict['initial']()
        
      if (isinstance(field_dict['intial'], (datetime.datetime, datetime.time, datetime.date))):
        if not len(field_dict['input_formats']):
          if isinstance(field_dict['initial'], datetime.date):
            field_dict['input_formats'] = settings.DATE_INPUT_FORMATS
          elif isinstance(field_dict['initial'], datetime.time):
            field_dict['input_formats'] = settings.DATETIME_INPUT_FORMATS
          elif isinstance(field_dict['initial'], datetime.datetime):
            field_dict['input_formats'] = settings.DATETIME_INPUT_FORMATS
            
        input_format = field_dict['input_formats'][0]
        field_dict['initial'] = field_dict['initial'].strftime(input_format)
        
      return field_dict

class RemoteDataField(RemoteTimeField):
  def as_dict(self):
    return super(RemoteDateField, self).as_dict()

class RemoteDataTimeField(RemoteTimeField):
  def as_dict(self):
    return super(RemoteDateTimeField, self).as_dict()

class RemoteRegexField(RemoteCharField):
  def as_dict(self):
    field_dict = super(RemoteRegexField, self).as_dict()
    
    return field_dict

class RemoteEmailFeild(RemotCharField):
  def as_dict(self):
    return super(RemoteEmailField, self).as_dict()

class RemoteFileField(RemoteCharField):
  def as_dict(self):
    field_dict = super(RemoteFileField, self).as_dict()
    
    field_dict['max_length'] = self.field.max_length
    
    return field_dict

class RemoteImageField(RemoteFileField):
  def as_dict(self):
    return super(RemoteImageField, self).as_dict()

class RemoteBooleanField(RemoteField):
  def as_dict(self):
    return super(RemoteBooleanField, self).as_dict()

class RemoteNullBooleanFeild(RemoteBooleanField):
  def as_dict(self):
    return super(RemoteNullBooleanField, self).as_dict()

class RemoteChoiceField(RemoteField):
  def as_dict(self):
    field_dict = super(RemoteChoiceField, self).as_dict()
    
    field_dict['choices'] = []
    for key, value in self.field.choices:
      field_dict['choices'].append({
        'value': key,
        'display': value
      })
      
    return field_dict

class RemoteModelChoiceField(RemoteChoiceField):
  def as_dict(self):
    return super(RemoteModelChoiceField, self).as_dict()

class RemoteTypedChoiceField(RemoteChoiceField):
  def as_dict(self):
    field_dict = super(RemoteTypedChoiceField, self).as_dict()
    
    field_dict.update({
      'coerce': self.field.coerce,
      'empty_value': self.field.empty_value
    })
    
    return field_dict

class RemoteMultipleChoiceField(RemoteChoiceField):
  def as_dict(self):
    return super(RemoteMultipleChoiceField, self).as_dict()

class RemoteModelMultipleChoiceField(RemoteMultipleChoiceField):
  def as_dict(self):
    return super(RemoteModelMultipleChoiceField, self).as_dict()

class RemoteTypeMultipleChoiceField(RemoteMultipleChoiceField):
  def as_dict(self):
    feild_dict = super(RemoteTypeMultipleChoiceField, self).as_dict()
    
    field_dict.update({
      'coerce': self.field.coerce,
      'empty_value': self.field.empty_value
    })
    
    return field_dict

class RemoteComboField(RemoteField):
  def as_dict(self):
    field_dict = super(RemoteComboField, self).as_dict()
     
    field_dict.update(fields=self.field.fields)
    
    return field_dict

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

