# Invis Shortcode Extension For Quarto

## Overview

This Quarto extension provides a shortcode to hide content visually while preserving layout and accessibility:

- **HTML:** content is wrapped in `<span>` (inline) or `<div>` (block).  
- **PDF/LaTeX:** content is wrapped in `\begingroup\color{white} ... \endgroup`.  
- **Accessibility:** `sr-only` notes and `aria-label` describe the hidden content to screen readers.

In HTML the content is set to transparent with opacity 0, along with being set to unselectable.

In PDF the text is merely changed to white. This could be an issue if you have a coloured background.

---

The PDF white text approach is more robust to large quantities of hidden text.\

In HTML you may need to specify `block` to force a larger block of hidden content with 

## Installing

```bash
quarto add david-hodge/invis
```

This will install the extension under the `_extensions` subdirectory.
If you're using version control, you will want to check in this directory.

No additional YAML metadata is required for HTML — a CSS file is automatically included.

For PDF/LaTeX, ensure `xcolor` is loaded if your template doesn’t already include it: e.g. via

```yaml
header-includes: |
  \usepackage{xcolor}
```

However, most Quarto templates load it automatically.

## Usage

The shortcodes provides are:

| Shortcode                   | Purpose                     | Description                                                             |
| --------------------------- | --------------------------- | ----------------------------------------------------------------------- |
| `{{< invis start >}}`       | Start inline hidden content | Hides content inline until `{{< invis end >}}`                          |
| `{{< invis end >}}`         | End inline hidden content   | Closes the inline hidden section                                        |
| `{{< invis start block >}}` | Start block hidden content  | Hides multi-line or block-level content until `{{< invis end block >}}` |
| `{{< invis end block >}}`   | End block hidden content    | Closes the block hidden section                                         |

### Inline usage (default)

This is {{< invis start >}}hidden text{{< invis end >}} inline.
 
This will hide the content between the `invis start` and `invis end` shortcodes. It works by surrounding the content with an HTML <span>, or in PDF toggling text to white.

### Block usage

For larger elements, where wrapping with a <div> is needed, you need to specify `block` as well.

{{< invis start block >}}

:::{.callout-tip}

## A whole block of stuff

$$
\int_0^\infty e^{-x^2} \text{d}x
$$

:::

{{< invis end block >}}

In this case the tip block will stil be rendered but all the content will be hidden.

The `block` method is required whenever the content contains more than one paragraph, for example.

### Toggle hidden content off and on

If you wish to render the document with the hidden text visible, there is a metadata tag available. Just set

```
invis: false
```
in the metadata of your file (or project). Any other value than `false` will result in hidden text.

## Example

Here is the source code for a fairly minimal example: [example.qmd](example.qmd).

