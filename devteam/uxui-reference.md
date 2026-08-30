# Dev team — UX/UI reference

REFERENCE class (structure-law): read when picking an input control, or when writing the design-system handover doc; the law lives in `uxui.md`.

## The control picker (UX-16)

Choose the input control by the shape of the choice, not by habit.

| Shape of the choice | Control |
|---|---|
| ≤5 mutually exclusive options | radio buttons or a segmented control (all options visible) |
| 6+ exclusive options | a select/dropdown |
| long or open-ended lists | searchable/typeahead select |
| multiple selections from few | checkboxes |
| multiple from many | multi-select with chips showing what's chosen |
| binary state | a toggle — and a toggle applies immediately; if it needs a Save, it should be a checkbox |

Every selection control shows its current state plainly and has a way to clear it.

## The design-system doc inventory (UX-8)

The handover doc carries:

- colour tone and palette roles — primary · surface · text · border · state colours
- the type scale and weights
- the spacing scale
- every shared component with its variants, its states (default / hover / disabled / loading / error), and when to use which

`UX REFERENCE LOADED`
