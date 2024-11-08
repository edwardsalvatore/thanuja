index=my_index "ah56718298"
| streamstats count as row_num
| streamstats current=f window=1 last(row_num) as next_row
| search next_row=row_num+1
