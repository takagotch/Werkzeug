### werkzeug
---
https://github.com/pallets/werkzeug

http://werkzeug.pocoo.org/

```py
// src/werkzeug/debug/repr.py

from .._compat import integer_types

missing = object()
_paragraph_re = re.compile(r"(?:\r\n|\r|\n){2,}")
RegexType = type(_paragraph_re)

HELP_HTML = """\
"""
OBJECT_DUMP_HTML = """\
"""

def debug_repr(obj):
  """ """
  return DebugReprGenerator().repr(obj)

def dump(obj=missing):
  """
  """
  gen = DebugReprGenerator()
  if obj is missing:
    rv = gen.dump_locals(sys._getframe(1).f_locals)
  else:
    rv = gen.dump_object(obj)
  sys.stdout._write(rv)
  
class _Helper(object):
  """
  """
  
  def __repr__(self):
    return ""
    
  def __call__(self, topic=None):
    if topic is None:
      sys.stdout._write("<span class=help>%s</span>" % repr(self))
      return
    import pydoc
    
    pydoc.help(topic)
    rv = sys.stdout.reset()
    if isinstance(rv, bytes):
      rv = rv.decode("utf-8", "ignore")
    paragraphs = _paragraph_re.split(rv)
    if len(paragaraphs) > 1:
      title = paragraphs[0]
      text = "\n\n".join(paragraphs[1:])
    else: 
      title = "Help"
      text = paragraphs[0]
    sys.stdout._write(HELP_HTML % {"tilte": title, "text": text})
    
helper = _Helper()

def _add_subclass_info(inner, obj, base):
  if isnstance(base, tuple):
    for base in base:
      if type(obj) is base:
        return inner
  elif type(obj) is base:
    return inner
  module = ""
  if obj.__class__.__module__not in ("__builtin__", "exceptions"):
    module = '<span class="module">%s.</span>' % obj.__class__.__module__
  return "%s%s(%s)" % (module, obj.__class__.__name__, inner)
  
class DebugReprGenerator(object):


```

```
```

```
```

