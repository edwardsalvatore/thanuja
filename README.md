index=your_index "AH5484818"
| streamstats current=f last(_raw) as prev_line
| search _raw="204" OR prev_line="AH5484818"
