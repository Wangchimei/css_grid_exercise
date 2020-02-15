# CSS Grid

#### CSS Grid Browser Support

Check on [Caniuse.com](https://caniuse.com/#feat=css-grid)

Quick jump to:
[Properties for the Parent](https://github.com/Wangchimei/css_grid_exercise#grid-properties-for-the-parent-grid-container)
[Properties for the Children](https://github.com/Wangchimei/css_grid_exercise#grid-properties-for-the-children-grid-items)

## Terminology

### Grid Container & Grid Items

```
<div class="container">
  <div class="item"></div>
  <div class="item">
  	<p class="sub-item"></p>
  </div>
  <div class="item"></div>
</div>
```

Using `display: grid` to trun its direct children into gird items.  
In this case, `<p class="sub-item"></p>` is not a grid item.

```
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}
```

### Grid Line

The dividing lines that make up the structure of the grid.  
They can be either vertical ("column grid lines") or horizontal ("row grid lines").

For example, `grid-template-columns: repeat(3, 1fr);` will show 3 columns, which means there are 4 grid lines (n + 1).

### Grid Track

The space between two adjacent grid lines.

### Grid Cell

A single "unit" of the grid.

### Grid Area

The total space surrounded by four grid lines.  
A grid area may be comprised of any number of grid cells.  
This is really useful of building a layout template.

## Grid Properties for the Parent (Grid Container)

### display

```
.container {
  display: grid | inline-grid;
}

```

- grid - generates a block-level grid
- inline-grid - generates an inline-level grid

### grid-template-columns & grid-template-rows

```
.container {
  grid-template-columns: 30% 20% 50%;
  grid-template-columns: 1fr 2fr 1fr;
  grid-template-columns: repeat(3, 1fr);
}
```

The value can be a length, a percentage, or a fraction of the free space in the grid (`fr` unit). e.g. `grid-template-rows: 25% 100px auto;`.

To define repeating patterns, use the `repeat()` notation to streamline things.  
`grid-template-columns: repeat(3, 1fr)` = `grid-template-columns: 1fr 1fr 1fr;`  
Note: The free space is calculated after any non-flexible items.  
`grid-template-columns: 1fr 50px 1fr 1fr;` : the total amount of free space available to the fr units doesn't include the 50px.

### grid-template-areas

Defines a grid template by referencing the names of the grid areas which are specified with the grid-area property.  
Repeating the name of a grid area causes the content to span those cells.  
A period `.` signifies an empty cell.

```
.item-a {
  grid-area: header;
}
.item-b {
  grid-area: main;
}
.item-c {
  grid-area: sidebar;
}
.item-d {
  grid-area: footer;
}

.container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: auto;
  grid-template-areas:
    "header header header header"
    "main main . sidebar"
    "footer footer footer footer";
}
```

### grid-template (Not recommeded)

A shorthand for setting `grid-template-columns`, `grid-template-rows`, and `grid-template-areas` in a single declaration.  
However, `grid-template` doesn't reset `grid-auto-columns`, `grid-auto-rows`, and `grid-auto-flow`, it's not recommended to use grid-template.

### grid-column-gap & grid-row-gap

Specifies the size of the grid lines, i.e. setting the width of the gutters between the columns/rows.

```
.container {
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: auto;
  grid-column-gap: 10px;
  grid-row-gap: 15px;
}
```

### grid-gap

A shorthand for `grid-row-gap` and `grid-column-gap`

```
.container {
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: auto;
  grid-gap: 10px;
}
```

### align-items & justify-items

**alignment for every grid item**

- align-items - aligns grid items along the column axis. (vertical)
- justify-items - aligns grid items along the row axis. (horizontal)

Values : `start`, `end`, `center`, `stretch`(default)

```
.container {
  align-items: start;
  justify-items: center;
}
```

### place-items

place-items sets both `align-items` and `justify-items` in a single declaration.  
The first value sets `align-items`, the second value `justify-items`.

```
.container {
  palce-items: start / center;
}
```

### justify-content & align-content

**alignment for the whole grid container**

- align-content - aligns the grid along the column axis. (vertical)
- justify-content - aligns the grid along the row axis. (horizontal)
  This is often seen if all of your grid items are sized with non-flexible units `px`, then you can set the alignment of the grid within the grid container.  
  Values : `start`, `end`, `center`, `stretch`, `space-around`, `space-between`, `space-evenly`

```
.container {
  align-content: center;
  justify-content: center;
}
```

### place-content

place-content sets both `align-content` and `justify-content` in a single declaration.
The first value sets `align-content`, the second value `justify-content`.

```
.container {
  palce-content: center / center;
}
```

### grid-auto-columns & grid-auto-rows

Specifies the size of any auto-generated grid tracks (aka implicit grid tracks).  
Implicit tracks get created when there are more grid items than cells in the grid or when a grid item is placed outside of the explicit grid.

```
.container {
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: minmax(150px, auto);
}
```

### grid-auto-flow

If you have grid items that you don't explicitly place on the grid, the auto-placement algorithm kicks in to automatically place the items.
Values: `row`, `column`, `row dense`, `column dense`

```
<section class="container">
  <div class="item-a">item-a</div>
  <div class="item-b">item-b</div>
  <div class="item-c">item-c</div>
  <div class="item-d">item-d</div>
  <div class="item-e">item-e</div>
</section>
```

```.item-a {
  grid-column: 1;
  grid-row: 1 / 3;
}
.item-e {
  grid-column: 5;
  grid-row: 1 / 3;
}
```

When `grid-auto-flow: row;`, item-b, item-c and item-d will flow across the available rows.

## Grid Properties for the Children (Grid Items)

### grid-column-start & grid-column-end

Determines a grid item's location within the grid by referring to specific grid lines.  
`grid-column-start` and `grid-column-end` specify where the item begins and ends on the column axis (vertical).

### grid-row-start & grid-row-end

Determines a grid item's location within the grid by referring to specific grid lines.  
`grid-row-start` and `grid-row-end` specify where the item begins and ends on the row axis (horizontal).

Items can overlap each other by using `z-index` to control their stacking order.

```
.item-a {
  grid-column-start: 1;
  grid-column-end: span 4;
  grid-row-start: 2;
  grid-row-end: span 2;
}
```

item-a is going to ...  
start from column line # 1 and span 4 grid cells (which is row line #5).
start from row line #2 and span 2 grid cells (which is row line #4).

### grid-column & grid-row

- grid-column - a shorthand for `grid-column-start` and `grid-column-end`
- grid-row - a shorthand for `grid-row-start` and `grid-row-end`

```
.item-a {
  grid-column: 1 / span 4;
  grid-row-start: 2 / 4;
}
```

### grid-area

1. Gives an item a name so that it can be referenced by a template created with the `grid-template-areas` property.

```
.item-a {
  grid-area: header;
}
.item-b {
  grid-area: main;
}
.item-c {
  grid-area: sidebar;
}
.item-d {
  grid-area: footer;
}

.container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: auto;
  grid-template-areas:
    "header header header header"
    "main main . sidebar"
    "footer footer footer footer";
}
```

2. Can be used as a shorthand for `grid-row-start` + `grid-column-start` + `grid-row-end` + `grid-column-end`.

```
.item-a {
  grid-column: 1 / span 4;
  grid-row-start: 2 / 4;
}
```

```
.item-a {
  grid-area: 2 / 1 / 4 / 5;
}
```

Note: `last-line`, `col4-start`, `-1` can also be used.

### justify-self & align-self

This value applies to a grid item inside a single cell.

- justify-self - aligns a grid item inside a cell along the row axis (vertial)
- align-self - aligns a grid item inside a cell along the column axis (horizontal)

Values : `start`, `end`, `center`, `stretch`(default)

```
.item-a {
  justify-self: start;
  align-self: center;
}
```

### place-self

place-self sets both `align-self` and `justify-self` in a single declaration.

```
.item-a {
  place-self: center start;
}
```
