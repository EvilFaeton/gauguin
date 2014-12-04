# Gauguin

Retrieves palette of main colors, merging similar colors using [YUV space](http://en.wikipedia.org/wiki/YUV).

## Why not just use `RMagick`?

How many colors do you recognize on the image below?

![Black and white image](spec/support/pictures/gray_and_black.png)

Many people would say `2`, but actually there are `1942`.

It's because of the fact that to make image more smooth, borders of the figure are not pure black but consist of many gray scale colors.

It's common that images includes very similar colors, so when you want to get useful color palette, you would need to process color histogram you get from `RMagick` yourself.

This gem was created to do this for you.

## Requirements

Gem depends on `RMagick` which requires `ImageMagick` to be installed.

### Ubuntu

    $ sudo apt-get install imagemagick

### OSX

    $ brew install imagemagick

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'gauguin'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install gauguin

## Usage

```ruby
Gauguin::Painting.new("path/to/image.png").palette
```

Result for image above would be:

```ruby
  {
    rgb(204, 204, 204)[0.5900935269505287] => [
      rgb(77, 77, 77)[7.383706620723603e-05],
      rgb(85, 85, 85)[0.00012306177701206005],
      # ...
      rgb(219, 220, 219)[1.2306177701206005e-05],
      rgb(220, 220, 220)[7.383706620723603e-05]
    ],
    rgb(0, 0, 0)[0.40990647304947003] => [
      rgb(0, 0, 0)[0.40990647304947003],
      rgb(1, 1, 1)[0.007912872261875462],
      # ...
      rgb(64, 64, 64)[6.153088850603002e-05],
      rgb(66, 66, 66)[6.153088850603002e-05]
    ]
  }
```

## Custom configuration

There are `4` parameters that you can configure:

- `max_colors_count` (default value is `10`) - maximum number of colors that palette will include
- `cut_off_limit` (default value is `1000`) - limit for the first cut off - if image has too many colors it is not efficient to calculate grouping for all of them, so only `cut_off_limit` of colors of the largest percentage are used
- `min_diff` (default value is `0.003`) - parameter used while calculating which colors should be ignored. Colors are sorted by percentage, then difference between percentages is calculated and only colors with difference greater than `min_diff` are taken to consideration
- `similarity_threshold` (default value is `50`) - maximum distance in [YUV space](http://en.wikipedia.org/wiki/YUV) to consider two colors as the same while grouping

To configure any of above options you can use configuration block.
For example changing `max_colors_count` would look like this:

```ruby
Gauguin.configuration do |config|
  config.max_colors_count = 7
end
```

## Contributing

1. Fork it ( https://github.com/LunarLogic/gauguin/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request
