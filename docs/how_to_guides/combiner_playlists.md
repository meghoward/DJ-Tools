# Combine Playlists with Boolean Algebra

In this guide you will learn how to automatically build playlists based off of powerful boolean algebra expressions that apply set operations to tags, playlists, and other selections of your Collection.

## Prerequisites

* [Rekordbox settings](../tutorials/getting_started/setup.md#importing-tracks-from-xml)
* [Get to Know Your Rekordbox Collection](../conceptual_guides/rekordbox_collection.md)
* [A light review of set theory](https://en.wikipedia.org/wiki/Set_theory#Basic_concepts_and_notation)

## Why combine playlists with boolean algebra?
It may not be commonly known, but Rekordbox already has a very elementary version of this functionality called the "Track Filter":
![alt text](../../images/Rekordbox_track_filter.png "Rekordbox Track Filter")

There are many shortcomings with the Track Filter which make it far inferior to the [Combiner][djtools.collection.helpers.build_combiner_playlists]:

1. Portability
    * can only be used on a laptop running Rekordbox (not available on XDJs, CDJs, etc.)
    * can only be used with the master database on a laptop (friends plugging in their devices lose access to this feature)
1. Operands
    * only works on `My Tags`, color, rating, key, and a maximum of 6% range from a single BPM value
1. Operators
    * can only apply `AND` and `OR` logic
    * can only apply logic to `My Tags`
    * can only apply one of these logic operators per grouping of `My Tags`

The `Combiner` solves all these issues in the following ways:

1. Portability
    * generates regular playlists which can be exported to a device for use on any Pioneer system
    * combining this with the [Copy Tracks From Playlists](../how_to_guides/copy_playlists.md) feature gives you portability on non-Pioneer systems
1. Operands
    * works on `My Tags` and `Genre` tags (and is extensible so other tags can be used as well)
    * includes selector syntax to choose arbitrary number of BPM / rating values as well as arbitrary ranges of values
    * includes selector syntax to choose playlists in your Collection
1. Operators
    * adds a `NOT` operator to take the set difference
    * allows you to apply logic operators to *any* of the operands, not just `My Tags`
    * allows you to construct arbitrarily complex expressions using parentheses to control the order of operations

## Syntax

* logical operators:
    - `AND` = `&`
    - `OR` = `|`
    - `NOT` = `~`
* playlist selectors:
    - `{playlist name}`
* BPM selectors (values > 5 are considered BPMs):
    - `[80, 90, 130-150]`
* rating selectors (values <= 5 are considered ratings):
    - `[0, 1, 3-5]`
* grouping:
    - `(`, `)`

## How it's done

1. Configure your desired playlists for the `Combiner` by constructing a boolean algebra expression with the syntax noted above
1. Run the command `--collection-playlists`
1. Import the `PLAYLIST_BUILDER` folder from the generated collection

## Example
As is done in the [Build Playlists From Tags](collection_playlists.md#example) how-to guide, we'll start by looking at some simple expressions configured in the pre-packaged [collection_playlists.yaml](https://github.com/a-rich/DJ-Tools/blob/main/djtools/configs/collection_playlists.yaml), but this time we'll focus only on the `Combiner` section:
![alt text](../../images/Rekordbox_playlists_yaml.png "Collection playlists YAML")

Note that the examples above are trivial ones designed to get 100% code coverage in unit tests.

This configuration contains a single named folder with a flat list of playlists. Each playlists' name is a boolean algebra expression that uses the syntax noted above to describe how different tags and selectors are to be unioned, intersected, and differenced to produce the final playlist of tracks. Valid expressions must contain at least two operands and must have one less operator than there are operands. 

Once you've finalized your playlist configuration, run the following command to build the playlists:

`djtools --collection-playlists`

Now you can import the `PLAYLIST_BUILDER` folder to load these playlists into your Collection.