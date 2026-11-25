# COCO 5k test split (image + caption pairs)

Self-contained export of the **Karpathy `split == "test"`** subset of MS-COCO.

## Counts

| item | value |
|---|---|
| images | 5,000 |
| captions | 25,010 |
| images with 6 captions | 10 |
| image bytes | 820.3 MB |

## Layout

```
images/                  5,000 jpg, original COCO file names
captions.jsonl           25,010 lines, one caption per line (columns below)
pairs.csv                same rows, UTF-8 CSV for Excel/pandas
captions_by_image.json   {"dataset","split","image_dir","images":[...]}, Karpathy-shaped
manifest.csv             5,000 rows: cocoid, filename, n_captions, size_bytes
```

## Fields

`captions.jsonl` / `pairs.csv` (one row per caption):

| field | type | meaning |
|---|---|---|
| `cocoid` | int | real COCO image id, e.g. 391895 (unique key) |
| `filename` | str | `COCO_val2014_000000XXXXXX.jpg` |
| `imgid` | int | Karpathy running index 0-123286, **not** a COCO id |
| `sentid` | int | caption id (unique key) |
| `caption` | str | `raw` with trailing whitespace stripped |
| `split` | str | always `test` |

`captions_by_image.json` keeps the original Karpathy fields (`filepath`, `filename`,
`sentids`, `imgid`, `cocoid`, `split`, `sentences[].tokens/raw/imgid/sentid`), so
existing Karpathy-based loaders can read it unchanged. The images now live under
`images/`, which is recorded in the top-level `image_dir` key.

## Provenance

- Split: `cs.stanford.edu/people/karpathy/deepimagesent/caption_datasets.zip`
- Images: COCO `val2014.zip` (https://cocodataset.org/#download), all test images
  come from val2014
- Captions: MS-COCO 2014 human captions, 5 per image (10 test images have 6)

## Notes

- `imgid` is a sequential index, `cocoid` is the COCO id; use the latter to join
  with official COCO annotation files.
- COCO images keep their original Flickr licenses. Redistribute image ids and
  annotations only; do not republish the images themselves.
