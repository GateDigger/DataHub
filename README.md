# DataHub
is a generic concept - a piece of software which accepts data entries from source systems, stores them, sorts and modifies them according to specified rules, and sends them to assigned target systems. This repo contains PL/SQL scripts which are capable of decorating an Oracle DB table by additional structure in order to facilitate this process. I shall call such decoration a "channel".

## Overview
Let there be a table, with a single-column primary key, which can store all data of interest. The attached PL/SQL scripts will perform the following operations:

### General tables, one collection can serve multiple channels
1. [01gl_create_entry_state_tbl.txt](src/01gl_create_entry_state_tbl.txt)
   - Creates a table of states the entries can be in as they pass through the DataHub
2. [02gl_create_external_system_tbl.txt](src/02gl_create_external_system_tbl.txt)
   - Creates a table of external systems the DataHub will interact with
3. [03gl_create_data_flow_tbl.txt](src/03gl_create_data_flow_tbl.txt)
   - Creates a table for bookkeeping of import and export processes
### Specific tables, one collection per channel
4. [04loc_decorate_main_tbl.txt](src/04loc_decorate_main_tbl.txt)
   - Decorates the main table with a few technical attributes
     - status - how far the row is in processing
     - status_dtl - comment on status
     - target_system - to which system the row shall be sent
     - source_obj - how it got in
     - target_obj - how it got / will get out
     - current_mod_src_type + current_mod_src - circumstances of row updates
5. [05loc_create_hist_tbl.txt](src/05loc_create_hist_tbl.txt)
   - Creates a table to keep track of modifications on the main table's data
   - Attributes are mostly self-explanatory, refer to the content of point 6 if in doubt
   - TODO: make the script automatically determine how long target_old_val and target_new_val have to be
6. [06loc_create_hist_trigger.txt](src/06loc_create_hist_trigger.txt)
   - Creates a trigger which will write to the table mentioned in point 5
7. [07loc_create_rule_tbl.txt](src/07loc_create_rule_tbl.txt)
   - Creates a table of rules
      - Rules will sort and modify pending entries in the main table
   - creation_date, active_from and active_to are self-explanatory, please refer to the content of point 9 for the rest
8. [08loc_create_rule_log_tbl.txt](src/08loc_create_rule_log_tbl.txt)
   - Creates a table designated for logging of rule application procedure executions
9. [09loc_create_rule_proc_package.txt](src/09loc_create_rule_proc_package.txt)
   - Creates a package consisting of the following procedures
      - Apply rules
      - Apply rules onwards from a rule of specified OID
      - Log start of an application procedure execution
      - Log end of an application procedure execution
  
Every script prints out the code it intends to execute; I encourage your caution and dilligence, use the comment functionality.

## License

MIT License

Copyright (c) 2023 GateDigger

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
