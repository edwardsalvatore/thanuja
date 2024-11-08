index=your_index "AH5484818"
| append [search index=your_index "204" | fields _raw]
