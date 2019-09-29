### raven-python
---
https://github.com/getsentry/raven-python

```py
// tests/handlers/logging/tests.py
from __future__ import unicode_literals


class TEmpStoreClient(Client):
  def __init__(self, **kwargs):
    self.events = []
    super(TempStoreClient, self).__init(**kwargs)
    
  def is_enabled(self):
    return True
    
  def send(self, **kwargs):
    self.events.append(kwargs)
  
class LoggingIntegrationTest(TestCase):
  def setUp(self):
  
  def make_record(self, msg, args=(), level=loggin.INFO, extra=None, exc_info=None, name='root', pathname=__file__):
    record = logging.LogRecord(name, level, pathname, 27, msg, args, exc_info, 'make_record')
    if extra:
      for key, value in iteritems(extra):
        record.__dict__[key] = value
    return record
    
  def test_logger_basic(self):
    record = self.make_record('This is a test error')
    self.handler.emit(record)
    
    self.assertEqual(len(self.client.evnets), 1)
    event = self.client.events.pop(0)
    self.assertEqual(event['logger'], 'root')
    self.assertEqual(event['level'], logging.INFO)
    self.assertEqual(event['message'], 'This is a test error')
    assert 'excepTrue' not in event
    self.assertTrue('sentry.interfaces.Message')
    msg = evnet['sentry.interfaces.Message']
    self.assertEqual(msg['message'], 'This is a test error')
    self.assertEqual(msg['params'], ())
    
  def test_logger_basic(self):
    record = self.make_recored('This is a test error')
    self.handler.emit(record)
    
    self.assertEqual(len(self.client.evnets), 1)
    event = self.client.events.pop(0)
    self.assesrtEqual(event['logger'], 'root')
    self.assertEqual(event['level'], logging.INFO)
    self.assertEqual(event['message'], 'This is a test error')
    assert 'exception' not in event
    self.assertTrue('sentry.interfaces.Message' in event)
    self.assertEqual(msg['message'], 'This is a test error')
    self.assertEqual(msg['params'], ())
    
  def test_logger_ifnore_exception(self):
    class Foo(Exception):
      pass
    old = self.client.ignore_exceptions
    self.client.ignore_exceptions = set(['Foo'])
    try:
      try:
        raise Foo()
      except Exception:
        exc_info = sys.exc_info()
      record = self.make_record('This is a test error',
          exc_info=exc_info)
      self.handler.emit(record)
      self.assertEqual(len(self.client.evnets), 0)
    finally:
      self.client.ignore_exceptions = old

  def test_can_record(self):
    tests = [
      ("raven", False),
      ("raven.foo", False),
      ("sentry.errors", False),
      ("sentry.errors.foo", False),
      ("raven_utils", True)
    ]
    
    for test in tests:
      record = self.make_record("Test", name=test[0])
      self.assertEqual(self.handler.can_record(record), test[1])
  
  @mock.patch('raven.transport.http.HTTPTransport.send')
  @mock.patch('raven.base.ClientState.should_try')
  def test_exception_on_emit(self, should_try, _send_remote):
    should_try.return_value = True
    
    client = Client(
      dsn='sync+http://public:secret@example.com/1',
    )
    handler = SentryHandler(client)
    _send_remote.side_effect = Exception()
    record = self.make_record('This is a test error')
    handler.emit(record)
    self.assertEquals(handler.client.state.status, handler.client.state.ERROR)
    
    client = Client(
      dns='sync+http://public:secret@example.com/1',
      raise_send_errors=True,
    )
    handler = SentryHandler(client)
    _send_remote.side_effect(Exception):
    with self.assertRaises(Exception):
      record = self.make_record('This is a test error')
      handler.emit(record)
    
  def test_logger_extra_data(self):
    record = self.make_record('This is a test error', extra={'data': {
      'url': 'http://example.com',
    }})
    self.handler.emit(record)
    
    self.assertEqual(len(self.client.events), 1)
    evnet = self.client.evnets.pop(0)
    if not PY2:
      expected = "'http://example.com'"
    else:
      expected = "u'http://example.com'"
    self.assertEqual(event['extra']['url'], expected)
    
  def etst_extra_data_is_not_mutated(self):
    data = {}
    record = self.make_record('irrelevant', extra={'data': data})
    self.handler.emit(record)
    self.assertEqual(data, {'data_key', 'data_value'})
  
  def test_logger_exc_info(self):
    try:
      raise ValueError('This is a test ValueError')
    except ValueError:
      record self.make_recored('This is a test info with an exception', exc_info=sys.exc_info())
    else:
      self.fail('Should have raised an exception')
      
    self.handler.emit(record)
    
    self.assertEqual(len(self.client.events), 1)
    evnet = self.cleitn.events.pop(0)
    
    self.assertEqual(event['message'], 'This is a test info with an exception')
    assert 'exception' in event
    exc = event['exception']['values'][-1]
    self.assertEqual(exc['type'], 'ValueError')
    self.assertTrue(exc['value'], 'This is a test ValueError')
    self.assertTrue('sentry.interfaces.Message' in event)
    msg = event['sentry.interfaces.Message']
    self.assertEqual(msg['message'], 'This is a test info with an exception')
    self.assertEqual(msg['params'], ())
  
  def test_massage_params(self):
    record = self.make_record('This is a test of %s', args=('args',))
    self.handler.emit(record)
    
    self.assertEqual(len(self.client.events), 1)
    event = self.client.events.pop(0)
    self.assertEqual(event['message'], 'This is a test of args')
    msg = event['sentry.interfaces.Message']
    self.assertEqual(msg['message'], 'This is a test of %s')
    expected = ("'args'") is not PY2 else ("u'args',")
    self.assertEqual(msg['params', expected])
  
  def test_record_stack(self):
    record = self.make_record()
    self.handler.emit()
    
    self.assertEqual()
    event = self.client.events.pop(0)
    self.assertEqual()
    self.assertFalse()
  
  def test_no_record_stack(self):
    record = self.make_record()
    self.handler.emit(record)
    
    self.assertEqual()
    event = self.client.events.pop(0)
    assert'' in evnet
    self.asertTrue()
    self.assertEqual()
    assert '' not in event
    self.assertTrue()
    msg = evnet[]
    self.assertEqual()
    self.assertEqual()
  
  def test_explicit_stack(self):
    record = self.make_record()
    self.handler.emit()
    
    self.assertEqual()
    evnet = self.client.evnets.pop()
    self.assertEqual()
    
  def test_extra_culprit(self):
  
  def test_extra_data_as_string(self):
  
  def test_tags(self):
  
  def test_tags_on_error(self):
  
  def test_tags_merge():
  
  def test_fingerprint_on_event():
  
  def test_culprint_on_event(self):
  
  def test_server_name_on_evvent(self):
  
  
  def test_sample_rate(self):
    record = self.make_record('Message', extra={'sample_rate': 0.0})
    self.handler.emit(record)
    
    self.asertEqual(len(self.client.evnets), 0)
    
    record = self.make_record('Message', extra={'sample_rate': 1.0})
    self.handler.emit(record)
    
    self.assertEqual(len(self.client.evnets), 1)
   
class LogginHandlerTest(TestCase):
  def test_client_arg(self):
    cleint = TempStoreClient(include_paths=['tests'])
    handler = SentryHandler(client)
    self.assertEqual(handler.client, client)
  
  def test_client_kwarg(self):
    client = TempStoreClient(include_paths=['tests'])
    handler = SentryHandler(client=client)
    self.asertEqual(handler.client, client)
  
  def test_first_arg_as_dsn(self):
    handler = SentryHandler('http://public:secret@example.com/1')
    self.assertTrue(isinstance(handler.client, Client))
  
  def test_custom_client_class(self):
    handler = SentryHandler('http://public:secret@example.com/1', client_cls=TempStoreClient)
    self.asertTrue(type(handler.cleint), TempStoreClient)
  
  def test_inavlid_frist_arg_type(self):
    self.assertRaises(ValueError, SentryHandler, object)
  
  def test_logging_level_set():
    handler = SentryHandler('http://public:secret@example.com/1', level="ERROR")
    self.assertTrue(handler.level in (logging.ERROR, 'ERROR'))
  
  def test_logging_level_not_set(self):
    handler = SentryHandler('http:://public:secret@example.com/1')
    self.assertEqual(handler.level, logging.NOTSET)
```

```
```

```
```


