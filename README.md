# sass-apca
Sass implementation of the [Accessible Perceptual Contrast Algorithm (APCA)](https://git.apcacontrast.com/) for the WCAG 3.0 specification.

## Installation

These scss functions are compatible with [sass (dart sass)](https://www.npmjs.com/package/sass) v1.33 and up.

```bash
npm install sass-apca
```

## Usage

### Contrast

APCA reports lightness contrast as an L<sup>c</sup> value from L<sup>c</sup> 0 to L<sup>c</sup> 106 for dark text on a light background, and L<sup>c</sup> 0 to L<sup>c</sup> -108 for light text on a dark background (dark mode). The minus sign merely indicates negative contrast, which means light text on a dark background. [^1]

| Parameter | Type | Description |
|:--- |:--- |:--- |
| $text-color | [Sass Color](https://sass-lang.com/documentation/values/colors) | Color of the text |
| $background-color | [Sass Color](https://sass-lang.com/documentation/values/colors) | Color of the background |

```scss
@use 'sass-apca/apca';

$contrast-black-on-white: apca.contrast(black, white); // 106.0406668287
$contrast-white-on-black: apca.contrast(white, black); // -107.8847261151
```

### Text color recommendation
Checks a light and a dark text color against a given background and returns the text color with the best contrast.

| Parameter | Type | Default | Description |
|:--- |:--- |:--- |:--- |
| $backgroundColor | [Sass color]() | | The background color to check against |
| $lightColor | [Sass color]() | white | [optional] A light text color |
| $darkColor | [Sass color]() | black | [optional] A dark text color |

```scss
@use 'sass-apca/apca';

body {
  background-color: #ccc;

  // either white or black text color
  color: apca.recommend-text-color(#ccc); // white

  // with custom light and dark text colors
  color: apca.recommend-text-color(#ccc, #eee, #111); // #111
}
```

## About contrast levels
These general levels are appropriate for use by themselves, without the need to reference a lookup table. APCA reports contrast as an L<sup>c</sup> value (lightness contrast) from L<sup>c</sup> 0 to L<sup>c</sup> 105+. For accessibility, consider L<sup>c</sup> 15 the point of invisibility for many users, and L<sup>c</sup> 90 is preferred for body text. [^2]

- **L<sup>c</sup> 90** • Preferred level for fluent text and columns of body text with a font no smaller than 14px/weight 400 (normal).
- **L<sup>c</sup> 75** • The minimum level for columns of body text with a font no smaller than 18px/400. L<sup>c</sup> 75 should be considered a minimum for text where readability is important.
- **L<sup>c</sup> 60** • The minimum level recommended for content text that is not body, column, or block text. In other words, text you want people to read. The minimums: 24px normal weight (400) or 16px/700 (bold). These values based on the reference font Helvetica.
- **L<sup>c</sup> 45** • The minimum for larger, heavier text (36px normal weight or 24px bold) such as headlines. This is also the minimum for pictograms with fine details.
- **L<sup>c</sup> 30** • The absolute minimum for any text not listed above. This includes placeholder text and disabled element text. This is also the minimum for large/solid semantic & understandable non-text elements.
- **L<sup>c</sup> 15** • The absolute minimum for any non-text that needs to be discernible and differentiable, and is no less than 6px in its smallest dimension. This may include disabled large buttons. Designers should treat anything below this level as invisible, as it will not be visible for many users. This minimum level should be avoided for any items important to the use, understanding, or interaction of the site.

## Roadmap
- Polarity function for figuring out if light or dark text has a higher contrast on any given background
- Compliance check for getting the current level of compliance of a given contrast ratio



## License

Code published in this repository licensed under the [MIT license](https://github.com/gfellerph/sass-apca/blob/main/LICENSE).

[^1]: APCA contrast calculator (https://www.myndex.com/APCA/)
[^2]: Why APCA (https://git.apcacontrast.com/documentation/WhyAPCA#use-case-ranges)