# vibegif (Python)

A small stdlib-only client for the vibegif catalog. Python 3.12+.

```
pip install "vibegif @ git+https://github.com/therapykit/vibegif.direct-img.link@<sha>#subdirectory=python"
```

It reads `catalog.json` from the site build (`public/catalog.json`, or the same file served from the site root). Assets sit next to it as `<id>.gif` and `<id>.webp`.

```python
from vibegif import Catalog

catalog = Catalog.load("public")            # a directory or a catalog.json file
catalog.get("brain/glow")                   # Item, or None
catalog.tags()                              # [("brain", 12), ...] most used first

for item in catalog.search("rainbow brain", tags=["glow"], section="psychedelic", limit=5):
    print(item.id, item.description)
    print(item.path(catalog.root, "webp"))  # local file
    print(item.url("https://vibegif.direct-img.link"))  # .gif URL
```

`tags` and `section` are hard filters (an item needs every tag). Query terms are scored per item: an exact tag beats a tag prefix, which beats a word in the description. Items matching no term are dropped, ties break by id, and an empty query returns the filtered set in id order.

Tests: `pip install -e ".[test]" && pytest`.
