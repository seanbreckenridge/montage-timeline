## montage-timeline

The core functionality here works, but its pretty clunky and uses lots of space in `/tmp` to preprocess each image input. This was created as a sort of CLI version of [topster](https://www.neverendingchartrendering.org/)

A wrapper for [`imagemagick`](https://imagemagick.org/index.php)'s `montage` command, used to create a collage from multiple images.

This accepts data to `STDIN` like:

```
https://img.sean.fish/i/demon_days_gorillaz_2005.jpeg|Item 1|subtitle text
/path/to/image.png|Item 2|subtitle text
```

... where the first path is an image (either a link or a local path). If its a link, this downloads/caches the image to `~/.cache/montage_timeline`, combines the subtitle text onto the image (by creating a temporary image in `/tmp`), then combines all the temporary images into a grid.

There are lots of environment variables one can set to override the default behaviour -- too many to list here, see [the script](./montage-timeline), but as an example, to remove all padding:

```bash
MONTAGE_EACH_IMG_PADDING=0 montage-timeline <input.txt
```

As an example, using [`my_feed`](https://github.com/seanbreckenridge/my_feed) API, converting the data to the input using `jq`:

```
curl -sL 'https://sean.fish/feed_api/data/?offset=0&limit=100&order_by=score&sort=desc&ftype=trakt_movie' \
  | jq -r '.[] | "\(.image_url)|\(.title)"' \
  | head -n 20 \
  | montage-timeline output.png
```

Creates the following image:

<img src="https://github.com/seanbreckenridge/montage-timeline/blob/master/.github/output.png?raw=true" width="500" />

## Install

Install `montage`, then copy the `montage-timeline` script onto your `$PATH`

Or use [`basher`](https://github.com/basherpm/basher):

```bash
basher install seanbreckenridge/montage-timeline
```
