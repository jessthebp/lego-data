# lego-data

colors.csv

One row per LEGO color.

Columns:

id: numeric color ID.

name: human‑readable color name (e.g., Black, Dark Turquoise).

rgb: hex RGB code (e.g., 05131D).

is_trans: whether the color is transparent/translucent.

In your Round 2 version: additional columns like num_parts, num_sets, y1, y2 (first/last year of use).

sets.csv

One row per LEGO set ever released.

Columns:

set_num: unique set identifier (e.g., 75911-1).

name: set name (e.g., “Space Mini-Figures”).

year: release year.

theme_id: foreign key to themes.csv.

num_parts: number of parts in the set.

themes.csv

One row per LEGO theme (product line or story world).

Columns:

id: theme ID.

name: theme name (e.g., Technic, Star Wars, City).

parent_id: if this theme belongs to a higher‑level parent theme.

inventories.csv

Links sets to inventories: a given set may have multiple inventories (versions).

Columns:

id: inventory ID.

version: version number.

set_num: set identifier (same as in sets.csv).

inventory_parts.csv

The core “parts in sets” table.

Columns:

inventory_id: inventory ID (points into inventories.csv).

part_num: part identifier (points into parts.csv).

color_id: color identifier (points into colors.csv).

quantity: number of copies of this part (in this color) in the inventory.

is_spare: whether this is a spare part.

inventory_sets.csv

Another way to link inventories back to sets (for multi‑inventory sets).

Columns:

inventory_id: inventory ID (from inventories.csv).

set_num: set identifier.

quantity: quantity of that inventory used.

parts.csv

One row per distinct LEGO part design.

Columns:

part_num: part ID (e.g., a specific brick or sticker sheet).

name: part name.

part_cat_id: foreign key to part_categories.csv.

(In your Round 2 data: part_material).

part_categories.csv

Types or categories of parts.

Columns:

id: category ID.

name: category name (Baseplates, Bricks Sloped, Duplo, etc.).

inventory_minifigs.csv

Maps inventories to minifigs they include.

Columns:

inventory_id: inventory ID.

fig_num: minifig ID (from minifigs.csv).

quantity: how many of that minifig are in the inventory.

minifigs.csv

One row per minifig design.

Columns:

fig_num: minifig ID.

name: minifig name (e.g., “Toy Store Employee”, “Customer Kid”).

num_parts: number of parts that make up the minifig.

img_url: image link.


The lego_eda_nlp.ipynb is basically going through three phases:

Loading and inspecting data

Read each CSV into a pandas DataFrame (colors, sets, themes, etc.).

Use .head(), .info(), and .describe() to understand columns, types, and distributions.

Example: count total colors, how many are transparent vs non‑transparent, and view first/last usage years.

Joining tables to explore relationships

Combine inventory_parts with inventories and sets to answer questions like:

“Which colors are used most, per year?”

“How many distinct colors does each set use?”

Combine sets with themes to see which themes dominate certain years or complexity ranges.

Combine inventory_minifigs with minifigs and inventory_parts to look at:

Color richness and complexity (number of parts) per minifig.

EDA & basic NLP/clustering

EDA:

Histograms of num_parts per set or minifig.

Trends over time (average pieces per set per year).

Scatterplots of num_parts vs num_distinct_colors.

NLP:

Token counts on name fields for sets, parts, minifigs, themes (how long/complex names are).

Keyword search for soccer/football, Star Wars, professions, kids/adults.

TF‑IDF + KMeans on minifig names to cluster them into semantic groups (e.g., city workers, fantasy characters).

Clustering:

StandardScaler + KMeans on features like num_parts and num_distinct_colors to group sets by complexity and palette.

Assign cluster labels back to the DataFrame to visualize how these clusters change by year or theme.

set_color_complexity: one row per set, with num_parts and num_distinct_colors.

set_complexity_age: one row per set, with a heuristic age_group_heuristic.

color_year_share: one row per (year, color), with share of parts that year.

theme_color_usage: one row per (theme, color), with total quantity.

minifig_color_complexity: one row per minifig, with color richness and num_parts.

minifigs_clusters: one row per minifig with a cluster label from TF‑IDF.

Providing simple Python EDA prototypes that show:

How LEGO’s complexity changes over time.

How palettes change over time.

How themes differ in their palettes and complexity.

How minifigs differ in their visual and structural detail.
