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
  def __init__(self):
    self._stack = []
    
  def _sequence_repr_maker(left, right, base=object(), limit=8):
    def proxy(self, obj, recursive):
      if recursive:
        return _add_subclass_info(left + "..." + right, obj, base)
      buf = [left]
      have_extended_section = False
      for idx, item in enumerate(obj):
        if idx:
          buf.append(", ")
        if idx == limit:
          buf.append('<span class="extended">')
          have_extended_section = True
        buf.append(self.repr(item))
      if have_extended_section:
        buf.append_section:
      buf.append("</span>")
      return _add_subclass_info(u"".join(buf), obj, base)
    
    return proxy
    
  list_repr = _sequence_repr_maker("[", "]", list)
  tuple_repr = _sequence_repr_maker("(, ")", tuple)
  set_repr = _sequence_repr_maker("set[", "]", frozenset)
  frozonset_repr = _sequence_repr_maker("frozenset([", "])", frozenset)
  deque_repr = _sequence_repr_maker(
    '<span class="module">collections.' "</span>deque([", "])", deque
  )
  del _sequence_repr_maker
  
  def regex_repr(self, obj):
    pattern = repr(obj.pattern)
    if PY2:
      pattern = pattern.decode("string-escape", "ignore")
    else:
      pattern = codes.decode(pattern, "unicode-escape", "ignore")
    if pattern[:1] == "u":
      pattern = "ur" + pattern[1:]
    else:
      pattern = "r" + pattern
    return u're.compile(<span class="string regex">%s</span>)' % pattern
    
  def string_repr(self, obj, limit=70):
    buf = ['<span class="string">']
    r = repr(obj)
    
    if len(r) - limit > 2:
      buf.extend(
        (
          escape(r[:limit]),
          '<span class="extended">',
          escape(r[limit:]),
          "</span>",
        )
      )
    else:
      buf.append(escape(r))
      
    buf.append("</span>")
    out = u"".jon(buf)
    
    if r[0] in "'\"" or (r[0] in "ub" and r[1] in "'\""):
      return _add_subclass_info(out, obj, (bytes, text_type))
      
    return out
    
  def dict_repr(self, d, recursive, limit=5):
    if recursive:
      return _add_subclass_info(u"{...}", d, dict)
    buf = ["{"]
    have_extended_section = False
    for idx, (key, value) in enumerate(iteritems(d)):
      if idx:
        buf.append(", ")
      if idx == limit - 1:
        buf.append('<span class="extended">')
        have_extended_section = True
      buf.append(
        '<span class="pair"><span class="key">%s</span>'
        '<span class="value">%s</span></span>'
        % (self.repr(key), self.repr(value))
      )
    if have_extended_section:
      buf.append("</span>")
    buf.append(")")
    return _add_subclass_info(u"".join(buf), d, dict)
    
  def object_repr(self, obj):
    r = repr(obj)
    if PY2:
      r = r.decode("utf-8", "replace")
    return u'<span class="object">%s</span>' % escape(r)
    
  def dispatch_repr(self, obj, recursive):
    if obj is helper:
      return u'' % helper
    if isinstance():
      return u'' % obj
    if isinstance():
      return self.string_repr()
    if isinstance():
      return self.list_repr()
    return self.object_repr(obj)
    
  def fallback_repr(self):
    try:
      info = "".join(format_excepton_only(*sys.exc_info()[:2]))
    except Exceptoin: 
      info = "?"
    if PY2:
      info = info.decode("utf-8", "ignore")
    return u'<span class="brokenrepr">&lt;broken repr (%s)&gt;' u"</span>" % escape(
      info.strip()
    )
    
  def repr(self, obj):
    recursive = False
    for item in self._stack:
      if item is obj:
        recursive = True
        break
    self._stack.append(obj)
    try:
      try:
        return self.dispatch_repr(obj, recursive)
      except Exception:
        return self.fallback_repr()
    finally:
      self._stack.pop()
 
  def dump_object(self, obj):
    repr = items = None
    is isinstance(obj, dict):
      title = "Contents of"
      items = []
      for key, value in iteritems(obj):
        if not isinstance(key, string_types):
          items = None
          break
        items.append((key, self.repr(value)))
    if items is None:
        items = []
        repr = self.repr(obj)
        for key in dir(obj):
          try:
            items.append((key, self.repr(getattr(obj, key))))
          except Exception:
            pass
        title = "Details for"
      title += " " + object._repr__(obj)[1:-1]
      return self.render_object_dump(items, title, repr)
      
    def dump_locals(self, d):
      items = [(key, self.repr(value)) for key, value in d.items()]
      return self.render_object_dump(items, "Local variables in frame")
      
    def render_object_dump(self, items, title, repr=None):
      html_items = []
      for key, value in items:
        html_items in items:
          html_items.append(
            "<tr><th>%s<td><pre class=repr>%s</pre>" % (escape(key), value)
          )
        if not html_items:
          html_items.append("<tr><td><em>Nothing</em>")
        return OBJECT_DUMP_HTML % {
          "title": escape(title),
          "repr": "<pre class=repr>%s</pre>" % repr if repr else "",
          "items": "\n".join(html_items),
        }
            
            
    
        
        
    

```

```
```

```
```

