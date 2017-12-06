# Magic Docs

<div id="documenter_content">

<section id="select_data">

<div class="page-header">

### 1\. Select Data.

* * *

</div>

**where (** _fields, value_ **)** - sets the condition of choice.  
Multiple calls are united all the conditions with the operator '**AND**'.  
_Option 1_**:** Accept the input field and value. The field should be indicated by a sign of comparison. Value will be automatically quoted.

1.  <span class="pln">$app-></span><span class="kwd">where</span><span class="pun">(</span><span class="str">'catid ='</span><span class="pun">,</span> <span class="lit">5</span><span class="pun">);</span>
2.  <span class="pln">$app-></span><span class="kwd">where</span><span class="pun">(</span><span class="str">'created >'</span><span class="pun">,</span> <span class="pln">$last_visit</span><span class="pun">);</span>
3.  <span class="com">// or</span>
4.  <span class="pln">$app-></span><span class="kwd">where</span><span class="pun">(</span><span class="str">'catid ='</span><span class="pun">,</span> <span class="lit">5</span><span class="pun">)-></span><span class="kwd">where</span><span class="pun">(</span><span class="str">'created >'</span><span class="pun">,</span> <span class="pln">$last_visit</span><span class="pun">);</span>

_Option 2:_ Accepts associations are an array of field-value. The field should be indicated by a sign of comparison. Value will be automatically quoted.

1.  <span class="pln">$cond</span> <span class="pun">=</span> <span class="pln">array</span><span class="pun">(</span><span class="str">'catid ='</span> <span class="pun">=></span> <span class="lit">5</span><span class="pun">,</span> <span class="str">'created >'</span> <span class="pun">=></span> <span class="pln">$last_visit</span><span class="pun">);</span>
2.  <span class="pln">$app-></span><span class="kwd">where</span><span class="pun">(</span><span class="pln">$cond</span><span class="pun">);</span>

<div class="alert">Please note: all the conditions in array will be merged with the operator 'AND'.</div>

_Option 3:_ Accepts custom line environment, you must specify the name of the table in front of each field, to avoid any conflict in relationships with other tables. You also have to take care of yourself on preparing values.

1.  <span class="pln">$app-></span><span class="kwd">where</span><span class="pun">(</span><span class="str">"content.catid = 5 AND content.created > '{$last_visit}'"</span><span class="pun">);</span>
2.  <span class="com">// or</span>
3.  <span class="pln">$app-></span><span class="kwd">where</span><span class="pun">(</span><span class="str">""</span><span class="pun">,</span> <span class="str">"content.catid = 5 AND content.created > '{$last_visit}'"</span><span class="pun">);</span> <span class="com">// 1.5 compat.</span>

_Alternative usage_

1.  <span class="com">// using OR glue</span>
2.  <span class="pln">$app-></span><span class="kwd">where</span><span class="pun">(</span><span class="str">'catid'</span><span class="pun">,</span> <span class="lit">5</span><span class="pun">);</span>
3.  <span class="pln">$app-></span><span class="pln">or_where</span><span class="pun">(</span><span class="str">'created >'</span><span class="pun">,</span> <span class="pln">$last_visit</span><span class="pun">);</span>
4.  <span class="pln"> </span>
5.  <span class="com">// using IN and NOT IN</span>
6.  <span class="pln">$app-></span><span class="kwd">where</span><span class="pun">(</span><span class="str">'catid'</span><span class="pun">,</span> <span class="pln">array</span><span class="pun">(</span><span class="str">'5'</span><span class="pun">,</span><span class="str">'7'</span><span class="pun">,</span><span class="str">'8'</span><span class="pun">);</span> <span class="com">// `catid` IN ('5','7','8')</span>
7.  <span class="pln">$app-></span><span class="kwd">where</span><span class="pun">(</span><span class="str">'catid !'</span><span class="pun">,</span> <span class="pln">array</span><span class="pun">(</span><span class="str">'5'</span><span class="pun">,</span><span class="str">'7'</span><span class="pun">,</span><span class="str">'8'</span><span class="pun">);</span> <span class="com">// `catid` NOT IN ('5','7','8')</span>

**or_where()** - the same as **where()** method, but multiple calls will be united with operator '**OR**'

**no_quotes(** _fields_ **)** - The method allows to cancel the automatic shielding values, so you can use the functions in mysql query. Affects on where expressions and pass_var () method. Takes comma-separated fieldnames or array of fieldnames in first parameter.

1.  <span class="pln">$app-></span><span class="pln">no_quotes</span><span class="pun">(</span><span class="str">'created'</span><span class="pun">);</span>
2.  <span class="pln">$app-></span><span class="pln">pass_var</span><span class="pun">(</span><span class="str">'created'</span><span class="pun">,</span><span class="str">'now()'</span><span class="pun">);</span>
3.  <span class="com">//or</span>
4.  <span class="pln">$app-></span><span class="kwd">where</span><span class="pun">(</span><span class="str">'created !='</span><span class="pun">,</span><span class="str">'null'</span><span class="pun">);</span>

**relation (** _field, target_table,target_id, target_name, where, order_by, multi, concat_separator, tree, depend_field, depend_on_ **)** - binds the current table with a list of the other table. Takes as input the name of the field in the current table, the name of the linked table, the link field, the title field , the selection condition for the linked table (optional).

1.  <span class="pln">$app-></span><span class="pln">relation</span><span class="pun">(</span><span class="str">'catid'</span><span class="pun">,</span><span class="str">'categories'</span><span class="pun">,</span><span class="str">'cid'</span><span class="pun">,</span><span class="str">'category_name'</span><span class="pun">,</span><span class="pln">array</span><span class="pun">(</span><span class="str">'published'</span> <span class="pun">=></span> <span class="lit">1</span><span class="pun">));</span>
2.  <span class="com">//or</span>
3.  <span class="pln">$app-></span><span class="pln">relation</span><span class="pun">(</span><span class="str">'catid'</span><span class="pun">,</span><span class="str">'categories'</span><span class="pun">,</span><span class="str">'cid'</span><span class="pun">,</span><span class="str">'category_name'</span><span class="pun">,</span><span class="str">'categories.published = 1'</span><span class="pun">);</span>

Since 1.5.4 relation() has additional parameters:

**relation (** _field, target_table, target_id, target_name, where_array, _order_by_, multi, concat_separator, tree, depend_field, depend_on_ **)**

*   _**field** -_ main table relation field, that will be replases; data will be written in this field
*   **_target_table_** - related (target) table, options for dropdown will be get from here
*   **_target_id_** - row id from target table, will be writen into _**field**_
*   **_target_name_** - field, that will be displayed as name of dropdown option. This can be array of few fields.
*   **_where_** - (optional) - allows to specify selection items from the **_target_table_**, see _where()_. Default is null.
*   **_order_by_** - (optional) - order by condition (eg. 'username desc'). Default is by target_name.
*   **_multi_** - (optional, boolean) - can change dropdown to multiselect (items will be saved separated by comma). Default is false.
*   **_concat_separator_** - (optional) - take effect only when **_target_name_** is array. Default is ' '.
*   **_tree_** - (optional) - array, sets tree rendering of dropdown list. Options: 1\. array('primary_key'=>'some_id_field_name','parent_key'=>'some_field_name') - primary and parent key field name, will be created pk tree. 2\. array('left_key'=>'some_field_name','level_key'=>'some_field_name') - left key and level field names, will be created nested sets tree.
*   **_depend_field_** - field from current table, options will be extracted based on parent field value ( like country_id column in cities table)
*   **_depend_on_** - field, thats will be parent to current dropdown.

<div class="alert alert-info">You can use {field_tags} in 'where' parameter to get variable from current row</div>

**fk_relation(**_label, field, fk_table, in_fk_field, out_fk_field, rel_tbl, rel_field, rel_name, rel_where, rel_orderby, rel_concat_separator, before, add_data_ **)** - allows to create, manage and display many-to-many connections. The syntax is similar to **relation().**

*   _**label** - Displaing field label (mus be unique, used as alias)_
*   _**field** - connection field from current table_
*   _**fk_table** - connection table_
*   _**in_fk_field** - field, connected with main table_
*   _**out_fk_field** - field, connected with relation table_
*   _**rel_tbl** -_ _relation table_
*   **_rel_field_ **- connection field from relation table
*   _**rel_name** -_ field, that will be displayed as name of dropdown option. This can be array of few fields.
*   _**rel_where** -_ (optional) - allows to specify selection items from the **_target_table_**, see _where()_. Default is null.
*   _**rel_orderby** _-_ (optional) -_ order by condition (eg. 'username desc'). Default is by _**rel_name.**_
*   _**rel_concat_separator** _-_ (optional) -_ take effect only when _**rel_name**_ is array. Default is ' '.
*   _**before** _-_ (optional) - if selected, field will be inserted before this field (by default - in the end)_
*   _**add_data** (array) _-_ (optional) - additional inserting data_

Structure of connections:

1.  ****<span class="pln">table</span>****
2.  <span class="pun">|-</span> <span class="pln">field</span> <span class="pun">--|</span>
3.  <span class="pun">|</span>
4.  <span class="pun">|</span> **<span class="pln">fk_table</span>**
5.  <span class="pun">|--</span> <span class="pun">|-</span> <span class="pln">in_fk_field</span>
6.  <span class="pun">|-</span> <span class="pln">out_fk_field</span> <span class="pun">--|</span>
7.  <span class="pun">|</span>
8.  <span class="pun">|</span> **<span class="pln">rel_table</span>**
9.  <span class="pun">|--</span> <span class="pun">|-</span> <span class="pln">rel_field</span>

**nested_table (** _inst_name, connect_field, nested_table, nested_connect_field_ **)** **-** takes instance name in first parameter, main field in second parameter, nested table in third and connection field from nested table in 4th.

Nested tables are using for easy editing of related records in other tables, such as the order and the goods in the order (see demo).

You can specify one nested table for each field of your main table. You can set options for nested tables as well as you do for the main table. Method **nested_table ()** creates an instance of a nested table, access to which can be obtained through the field name (specified in the first parameter of the method nested_table ()), only add to it the prefix nested_.

1.  <span class="pln">$app-></span><span class="pln">table</span><span class="pun">(</span><span class="str">'orders'</span><span class="pun">);</span> <span class="com">// main table</span>
2.  <span class="pln">$products_list</span> <span class="pun">=</span> <span class="pln">$app-></span><span class="pln">nested_table</span><span class="pun">(</span><span class="str">'products_list'</span><span class="pun">,</span><span class="str">'orderNumber'</span><span class="pun">,</span><span class="str">'orderdetails'</span><span class="pun">,</span><span class="str">'orderNumber'</span><span class="pun">);</span> <span class="com">// nested table</span>
3.  <span class="pln">$products_list</span><span class="pun">-></span><span class="pln">unset_add</span><span class="pun">();</span> <span class="com">// nested table instance access</span>

**search_pattern( '%', '%' )** - allows to define search pattern for grid search. Method replaces default param ($search_pattern) from configuration.

#### Table joining

magic provide table joining and manipulating with a few tables in one box. You can use extended field syntax, when you type field name (column name or alias) as table and field with dot separator (sample: $app->change_type('order.total', 'price');). You no need to use this when you not use table joining.

**join(** _field, joined_table, join_on_field [, alias] [, not_insert]_ **)**

_field_ - field from current table to join; _joined_table -_ table, that is joined; _join_on_field_ - joining on this column from joined table; _alias_ - (optional) - need to use when you join one tabes more than one times.

_not_insert_ - allows to disable inserting and deleting rows from joined table.

1.  <span class="pln">$app-></span><span class="pln">table</span><span class="pun">(</span><span class="str">'users'</span><span class="pun">);</span>
2.  <span class="pln">$app-></span><span class="pln">join</span><span class="pun">(</span><span class="str">'id'</span><span class="pun">,</span><span class="str">'profiles'</span><span class="pun">,</span><span class="str">'user_id'</span><span class="pun">);</span> <span class="com">// join users and profiles on users.id = profiles.user_id</span>
3.  <span class="pln"> </span>
4.  <span class="com">// now join 'profiles' and 'tokens' tables</span>
5.  <span class="pln">$app-></span><span class="pln">join</span><span class="pun">(</span><span class="str">'profiles.token_id'</span><span class="pun">,</span><span class="str">'tokens'</span><span class="pun">,</span><span class="str">'id'</span><span class="pun">);</span> <span class="com">// on profile.token_id = tokens.id</span>
6.  <span class="pln"> </span>
7.  <span class="com">// simple actions with fields: default and joined</span>
8.  <span class="pln">$app-></span><span class="pln">column</span><span class="pun">(</span><span class="str">'username'</span><span class="pun">,</span><span class="str">'email'</span><span class="pun">,</span><span class="str">'profile.city'</span><span class="pun">,</span><span class="str">'tokens.created'</span><span class="pun">);</span>

<div class="alert">**join** uses INNER JOIN, thats mean in all tables must be present joined rows. **join** connects only one row from table, joined on fields must be unique. When joined rows in not unique magic will be worked incorrect.</div>

#### Custom SQL

**query(** _sql_query_ **)** - custom sql query. This method allow you to use custom sql query and display **read-only** datagrid.

1.  <span class="pln">$app</span> <span class="pun">=</span> <span class="typ">Magic::</span><span class="pln">get_instance</span><span class="pun">();</span>
2.  <span class="pln">$app-></span><span class="pln">query</span><span class="pun">(</span><span class="str">'SELECT * FROM users WHERE age > 25'</span><span class="pun">);</span>
3.  <span class="pln">echo $app-></span><span class="pln">render</span><span class="pun">();</span>

</section>

<section id="overview_table">

<div class="page-header">

### 2\. Overview table.

* * *

</div>

**columns (** _columns, reverse_ **)** - sets the columns that you want to see in the review table. Takes a string, separated by commas, or an array of values. The second optional parameter causes the function to do the opposite, ie hides the selected columns. Takes **tru / false**, default **false**.

1.  <span class="pln">$app-></span><span class="pln">columns</span><span class="pun">(</span><span class="str">'name,email,city'</span><span class="pun">);</span>
2.  <span class="com">//or</span>
3.  <span class="pln">$app-></span><span class="pln">columns</span><span class="pun">(</span><span class="pln">array</span><span class="pun">(</span><span class="str">'name'</span><span class="pun">,</span><span class="str">'email'</span><span class="pun">,</span><span class="str">'city'</span><span class="pun">));</span>
4.  <span class="pln"> </span>
5.  <span class="com">// hide columns</span>
6.  <span class="pln">$app-></span><span class="pln">columns</span><span class="pun">(</span><span class="str">'name,email,city'</span><span class="pun">,</span> <span class="kwd">true</span><span class="pun">);</span>

**order_by (** _name, direction_ **)** - sets the initial sorting of the table, take the field and sort order. By default - in ascending order ('asc').

1.  <span class="pln">$app-></span><span class="pln">order_by</span><span class="pun">(</span><span class="str">'name'</span><span class="pun">,</span><span class="str">'desc'</span><span class="pun">);</span>

**label (** _fieldname, your_label_ **)** - Allows you to specify the name of your columns and fields, takes the field and the field name or an array.

1.  <span class="pln">$app-></span><span class="pln">label</span><span class="pun">(</span><span class="str">'name'</span><span class="pun">,</span><span class="str">'Your Name'</span><span class="pun">);</span>
2.  <span class="com">//or</span>
3.  <span class="pln">$app-></span><span class="pln">label</span><span class="pun">(</span><span class="pln">array</span><span class="pun">(</span><span class="str">'name'</span> <span class="pun">=></span> <span class="str">'Your Name'</span><span class="pun">));</span>

**column_name(** _fieldname, your_label_ **)** - the same as **label()**, but affects only on columns in grid.

**show_primary_ai_column (** _bool_ **)** - takes true / false, setting the display of primary auto-increment columns in the overview table.

1.  <span class="pln">$app-></span><span class="pln">show_primary_ai_column</span><span class="pun">(</span><span class="kwd">true</span><span class="pun">);</span>

**limit (** _int_ **)** - sets the initial limit for display tables, takes an integer in the first argument. The value set by this function is always available in the list to select the number of lines per page.

1.  <span class="pln">$app-></span><span class="pln">limit</span><span class="pun">(</span><span class="lit">25</span><span class="pun">);</span>

**limit_list (** _string_or_array_ **)** - specifies a list of values for the limits list. Takes an array of values, or string. Option 'all' lift a moderate amount of a result (display all rows).

1.  <span class="pln">$app-></span><span class="pln">limit_list</span><span class="pun">(</span><span class="str">'5,10,15,all'</span><span class="pun">);</span>
2.  <span class="com">//or</span>
3.  <span class="pln">$app-></span><span class="pln">limit_list</span><span class="pun">(</span><span class="pln">array</span><span class="pun">(</span><span class="str">'5'</span><span class="pun">,</span><span class="str">'10'</span><span class="pun">,</span><span class="str">'15'</span><span class="pun">,</span><span class="str">'all'</span><span class="pun">));</span>

**table_name (** _name, tooltip, tooltip_icon_ **)** - Changes the name of the table in the title, takes a string as the first parameter, tooltip text in second parameter (optional) and tooltip icon name in 3rd (optional)

1.  <span class="pln">$app-></span><span class="pln">table_name</span><span class="pun">(</span><span class="str">'My custom table!'</span><span class="pun">);</span>
2.  <span class="pln">$app-></span><span class="pln">table_name</span><span class="pun">(</span><span class="str">'My custom table!'</span><span class="pun">,</span> <span class="str">'Some tooltip text'</span><span class="pun">);</span>
3.  <span class="pln">$app-></span><span class="pln">table_name</span><span class="pun">(</span><span class="str">'My custom table!'</span><span class="pun">,</span> <span class="str">'Some tooltip text'</span><span class="pun">,</span><span class="str">'icon-leaf'</span><span class="pun">);</span>

**unset_add(** _true_ **)** - hides add button from list view.

1.  <span class="pln">$app-></span><span class="pln">unset_add</span><span class="pun">();</span>

**unset_edit(** _true_ **)** - hide edit button from list view.

1.  <span class="pln">$app-></span><span class="pln">unset_edit</span><span class="pun">();</span>

**unset_view(** _true_ **)** - hides view button from list view.

1.  <span class="pln">$app-></span><span class="pln">unset_view</span><span class="pun">();</span>

**unset_remove(** _true_ **)** - hide remove button from list view.

1.  <span class="pln">$app-></span><span class="pln">unset_remove</span><span class="pun">();</span>

**unset_csv(** _true_ **)** - hides csv-export button from list view.

1.  <span class="pln">$app-></span><span class="pln">unset_csv</span><span class="pun">();</span>

**unset_search(** _true_ **)** - hides search feature.

1.  <span class="pln">$app-></span><span class="pln">unset_search</span><span class="pun">();</span>

**unset_print(** _true_ **)** - hides printout feature.

1.  <span class="pln">$app-></span><span class="pln">unset_print</span><span class="pun">();</span>

**unset_title(** _true_ **)** - hides table title.

1.  <span class="pln">$app-></span><span class="pln">unset_title</span><span class="pun">();</span>

**unset_numbers(** _true_ **)** - hides rows numbers.

1.  <span class="pln">$app-></span><span class="pln">unset_numbers</span><span class="pun">();</span>

**unset_pagination(** _true_ **)** - hides pagination

1.  <span class="pln">$app-></span><span class="pln">unset_pagination</span><span class="pun">();</span>

**unset_limitlist(** _true_ **)** - hides list with limits buttons or dropdown

1.  <span class="pln">$app-></span><span class="pln">unset_limitlist</span><span class="pun">();</span>

**unset_sortable(** _true_ **)** - makes columns unsortable

1.  <span class="pln">$app-></span><span class="pln">unset_sortable</span><span class="pun">();</span>

**unset_list(** _true_ **)** - turn of grid view. Only details can be viewed or edited. Don't forget to set view parameter in **render()** method.

1.  <span class="pln">$app-></span><span class="pln">unset_list</span><span class="pun">();</span>

<div class="alert alert-success">**unset_view(), unset_edit(), unset_remove(), duplicate_button()** - this methods can get additional condition parameters (with {field_tags})</div>

1.  <span class="pln">$app-></span><span class="pln">unset_edit</span><span class="pun">(</span><span class="kwd">true</span><span class="pun">,</span><span class="str">'username'</span><span class="pun">,</span><span class="str">'='</span><span class="pun">,</span><span class="str">'admin'</span><span class="pun">);</span> <span class="com">// 'admin' row can't be editable</span>

**remove_confirm(** _true_ **)** - removes confirmation window on remove action. Takes true / false in the first parameter.

1.  <span class="pln">$app-></span><span class="pln">remove_confirm</span><span class="pun">(</span><span class="kwd">false</span><span class="pun">);</span>

**start_minimized (** _true_ **)** - start magic instance minimized. Takes true / false in the first parameter.

1.  <span class="pln">$app-></span><span class="pln">start_minimized</span><span class="pun">(</span><span class="kwd">true</span><span class="pun">);</span>

**benchmark (** _true_ **)** - displays information about the performance in the lower right corner of the magic window. Takes true / false in the first parameter.

1.  <span class="pln">$app-></span><span class="pln">benchmark</span><span class="pun">(</span><span class="kwd">true</span><span class="pun">);</span>

**column_cut (** _int_**,** _[fields]_ **)** - sets the maximum number of characters to be displayed in columns. Takes an integer value in the first parameter. In the second parameter you can define target field(s)

1.  <span class="pln">$app-></span><span class="pln">column_cut</span><span class="pun">(</span><span class="lit">30</span><span class="pun">);</span> <span class="com">// all columns</span>
2.  <span class="pln">$app-></span><span class="pln">column_cut</span><span class="pun">(</span><span class="lit">30</span><span class="pun">,</span><span class="str">'title,description'</span><span class="pun">);</span> <span class="com">// separate columns</span>

**duplicate_button (** _true_ **)** - show duplicate button. You can duplicate only the records in those tables that have auto-incremental primary field, and have no other unique indexes. Otherwise you will get an error.

1.  <span class="pln">$app-></span><span class="pln">duplicate_button</span><span class="pun">();</span>

**links_label(** _label_ **)** - creates label for links in grid view. Takes new label in first parameter

1.  <span class="pln">$app-></span><span class="pln">links_label</span><span class="pun">(</span><span class="str">'home url'</span><span class="pun">);</span>
2.  <span class="com">// or</span>
3.  <span class="pln">$app-></span><span class="pln">links_label</span><span class="pun">(</span><span class="str">'</span><span class="tag"><span class="str"><i</span></span> <span class="atn"><span class="str">class</span></span><span class="pun"><span class="str">=</span></span><span class="atv"><span class="str">"</span></span><span class="str">icon-home</span><span class="atv"><span class="str">"</span></span><span class="tag"><span class="str">></i></span></span><span class="str">'</span><span class="pun">);</span> <span class="com">// bootstrap icon for bootstrap theme</span>

**emails_label(** _label_ **)** - creates label for links in grid view. Takes new label in first parameter

1.  <span class="pln">$app-></span><span class="pln">emails_label</span><span class="pun">(</span><span class="str">'Contact email'</span><span class="pun">);</span>

**sum (** _columns, classname, custom_data_ **)** - calculates sum for columns and shows result row in the bottom of table. Calculates sum of the entire list, regardless of pagination. Takes columns list in first parameter, optional classname in second, and optional custom text pattern in third.

1.  <span class="pln">$app-></span><span class="pln">sum</span><span class="pun">(</span><span class="str">'price,fee,quantity'</span><span class="pun">);</span>
2.  <span class="com">//or</span>
3.  <span class="pln">$app-></span><span class="pln">sum</span><span class="pun">(</span><span class="pln">array</span><span class="pun">(</span><span class="str">'price'</span><span class="pun">,</span><span class="str">'fee'</span><span class="pun">,</span><span class="str">'quantity'</span><span class="pun">));</span>
4.  <span class="com">//or</span>
5.  <span class="pln">$app-></span><span class="pln">sum</span><span class="pun">(</span><span class="str">'price'</span><span class="pun">,</span><span class="str">'align-center'</span><span class="pun">,</span><span class="str">'Total price is {value}'</span><span class="pun">);</span> <span class="com">// use {value} tag to get sum value in pattern</span>

**button(** _link, title, icon, classname, array_of_attributes_**,** _condition_array_ **)** - adds custom link in grid, like edit or remove. You can define url in first parameter (required), name (optional) in 2nd, icon(optional) in 3rd, class attribute (optional) in 4th, additional button attributes (assoc array) as 5th.

For icon field you must use predefined classes, this is **Icon glyphs** for bootstrap theme (e.g. icon-glass, icon-music...).

1.  <span class="pln">$app-></span><span class="pln">button</span><span class="pun">(</span><span class="str">'http://example.com'</span><span class="pun">);</span>
2.  <span class="pln">$app-></span><span class="pln">button</span><span class="pun">(</span><span class="str">'http://example.com'</span><span class="pun">,</span><span class="str">'My Title'</span><span class="pun">,</span><span class="str">'</span><span class="box1"><span class="str">icon-link</span></span><span class="str">'</span><span class="pun">,</span><span class="str">''</span><span class="pun">,</span><span class="pln">array</span><span class="pun">(</span><span class="str">'target'</span><span class="pun">=></span><span class="str">'_blank'</span><span class="pun">));</span>
3.  <span class="pln"> </span>
4.  <span class="com">// {column_tags} usage:</span>
5.  <span class="pln">$app-></span><span class="pln">button</span><span class="pun">(</span><span class="str">'http://example.com/{user_id}/?token={user_token}'</span><span class="pun">);</span>

<div class="alert alert-info">You can use buttons with text labels. See $button_labels parameter in configuration file</div>

Also button() supports simple condition in 6th parameter. Condition value supports {field_tags}. Example:

1.  <span class="com">// show button whith link from 'link' field when 'link' field is not empty</span>
2.  <span class="pln">$app-></span><span class="pln">button</span><span class="pun">(</span><span class="str">'{link}'</span><span class="pun">,</span><span class="str">'userlink'</span><span class="pun">,</span><span class="str">'link'</span><span class="pun">,</span><span class="str">''</span><span class="pun">,</span><span class="str">''</span><span class="pun">,</span><span class="pln">array</span><span class="pun">(</span><span class="str">'link'</span><span class="pun">,</span><span class="str">'!='</span><span class="pun">,</span><span class="str">''</span><span class="pun">));</span>

**highlight(** _field, operator, value, color, classname_ **)** - adds background color or class attribute for grid cell based on user's condition.

1.  <span class="pln">$app-></span><span class="pln">highlight</span><span class="pun">(</span><span class="str">'orderNumber'</span><span class="pun">,</span><span class="str">'='</span><span class="pun">,</span><span class="str">'10101'</span><span class="pun">,</span><span class="str">'red'</span><span class="pun">);</span>
2.  <span class="pln">$app-></span><span class="pln">highlight</span><span class="pun">(</span><span class="str">'orderNumber'</span><span class="pun">,</span><span class="str">'>='</span><span class="pun">,</span><span class="str">'10113'</span><span class="pun">,</span><span class="str">'#87FF6C'</span><span class="pun">);</span>
3.  <span class="pln">$app-></span><span class="pln">highlight</span><span class="pun">(</span><span class="str">'city'</span><span class="pun">,</span><span class="str">'='</span><span class="pun">,</span><span class="str">'Madrid'</span><span class="pun">,</span><span class="str">''</span><span class="pun">,</span><span class="str">'main-city'</span><span class="pun">);</span> <span class="com">// you can define class attribute</span>

Also you can get value from current row using {field_tag}

1.  <span class="pln">$app-></span><span class="pln">highlight</span><span class="pun">(</span><span class="str">'sum'</span><span class="pun">,</span> <span class="str">'>'</span><span class="pun">,</span> <span class="str">'{profit}'</span><span class="pun">,</span> <span class="str">'red'</span><span class="pun">);</span>

**highlight_row****(** _field, operator, value, color, classname_ **)** - the same as **highlight()**, but full row will be highlighted

**column_class(** _column(s), classname_ **)** -  adds class atribute to column(s).

1.  <span class="pln">$app-></span><span class="pln">column_class</span><span class="pun">(</span><span class="str">'price,sum,count'</span><span class="pun">,</span> <span class="str">'align-center'</span><span class="pun">);</span>

Predefined classes: **align-left, align-right, align-center, font-bold, font-italic, text-underline**

**subselect(** _column_name, query, before_column_ **)** - select to other table with parameters. This will create new column and inserts it after last column in table, or before colum defined in 3rd parameter

1.  <span class="com">//subselect</span>
2.  <span class="pln">$app-></span><span class="pln">subselect</span><span class="pun">(</span><span class="str">'Order total'</span><span class="pun">,</span><span class="str">'SELECT SUM(priceEach) FROM orderdetails WHERE orderNumber = {orderNumber}'</span><span class="pun">);</span> <span class="com">// insert as last column</span>
3.  <span class="pln">$app-></span><span class="pln">subselect</span><span class="pun">(</span><span class="str">'Products count'</span><span class="pun">,</span><span class="str">'SELECT COUNT(*) FROM orderdetails WHERE orderNumber = {orderNumber}'</span><span class="pun">,</span><span class="str">'status'</span><span class="pun">);</span> <span class="com">// insert this column before 'status' column</span>
4.  <span class="pln"> </span>
5.  <span class="com">// you can use order() and change_type() for this columns;</span>
6.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'Order total'</span><span class="pun">,</span><span class="str">'price'</span><span class="pun">,</span><span class="str">''</span><span class="pun">,</span><span class="pln">array</span><span class="pun">(</span><span class="str">'prefix'</span><span class="pun">=></span><span class="str">'/span></span><span class="pun">));</span>
7.  <span class="pln">$app-></span><span class="pln">order_by</span><span class="pun">(</span><span class="str">'Products count'</span><span class="pun">);</span>

Also you can operate with fields only in current row

1.  <span class="pln">$app-></span><span class="pln">subselect</span><span class="pun">(</span><span class="str">'Sum'</span><span class="pun">,</span><span class="str">'{price}*{qty}'</span><span class="pun">);</span>

**modal(** _field(s), icon_ **)** - shows cell info in modal.

1.  <span class="pln">$app-></span><span class="pln">modal</span><span class="pun">(</span><span class="str">'customerName,customerDescription);</span>
2.  <span class="str">$app->modal('</span><span class="pln">customerName</span><span class="str">', '</span><span class="pln">icon</span><span class="pun">-</span><span class="pln">user</span><span class="str">');</span>
3.  <span class="str">$app->modal(array('</span><span class="pln">customerName</span><span class="str">'=>'</span><span class="pln">icon</span><span class="pun">-</span><span class="pln">user</span><span class="str">'));</span>

**column_pattern**(column_name, pattern_code)  - replaces default column cell output by custom pattern. Pattern can contain {field_tags} and {value} tag (value of current column)

1.  <span class="pln">$app-></span><span class="pln">column_pattern</span><span class="pun">(</span><span class="str">'username'</span><span class="pun">,</span><span class="str">'My name is {value}'</span><span class="pun">);</span>

Differense between {value} and {username} (see example): {username} will return raw value from current cell, but {value} will return full output if your field has some extra features (like image or formatted price)

**field_tooltip(** _field(s), tooltip_text  [, icon ]_ **)** - creates tooltip icon for field label in create/edit/view mode.

1.  <span class="pln">$app-></span><span class="pln">field_tooltip</span><span class="pun">(</span><span class="str">'productName'</span><span class="pun">,</span><span class="str">'Enter product name here'</span><span class="pun">);</span>

**column_tooltip(** _column(s), tooltip_text  [, icon ]_ **)** - creates tooltip icon for column label in create/edit/view mode.

1.  <span class="pln">$app-></span><span class="pln">field_tooltip</span><span class="pun">(</span><span class="str">'productName'</span><span class="pun">,</span><span class="str">'Enter product name here'</span><span class="pun">);</span>

**search_columns(** _[ column(s) ] [, default_column ]_ **)** - defines column list for search and default search column

1.  <span class="pln">$app-></span><span class="pln">search_columns</span><span class="pun">(</span><span class="str">'productVendor,quantityInStock,buyPrice'</span><span class="pun">,</span><span class="str">'quantityInStock'</span><span class="pun">);</span>

**buttons_position(** _position_ **)** - changes position of grid buttons. Can be '**left**', '**right**' or '**none**'. 'None' option will hide buttons, their features will be available (unlike of unset_ methods). Default is **'right'** and can be changed in configuration file.

1.  <span class="pln">$app-></span><span class="pln">buttons_position</span><span class="pun">(</span><span class="str">'left'</span><span class="pun">);</span>

**hide_button(** _button_name(s)_ **)** - hides system or your custom button ( defined with render_button() method ). This not disables button feature (unlike of unset_ methods).

1.  <span class="pln">$app-></span><span class="pln">hide_button</span><span class="pun">(</span><span class="str">'save_return'</span><span class="pun">);</span>

Default system buttons are: _view, edit, remove, duplicate, add, csv, print, save_new, save_edit, save_return, return_.

**column_width(** _column(s), width_ **)** - sets width of magic columns manualy.

1.  <span class="pln">$app-></span><span class="pln">column_width</span><span class="pun">(</span><span class="str">'description'</span><span class="pun">,</span><span class="str">'65%'</span><span class="pun">);</span>
2.  <span class="pln">$app-></span><span class="pln">column_width</span><span class="pun">(</span><span class="str">'first_name,last_name'</span><span class="pun">,</span><span class="str">'100px'</span><span class="pun">);</span>

</section>

<section id="create_edit">

<div class="page-header">

### 3\. Create / Edit

* * *

</div>

**fields (** _field(s), reverse, tab_name, mode_ **)** - the table fields that you want to see when you edit or create entries. Takes a string, separated by commas, or an array of values. The second optional parameter causes the function to do the opposite, ie hides the selected fields. Takes **tru / false**, default **false**.

1.  <span class="pln">$app-></span><span class="pln">fields</span><span class="pun">(</span><span class="str">'name,email,city'</span><span class="pun">);</span>
2.  <span class="com">//or</span>
3.  <span class="pln">$app-></span><span class="pln">fields</span><span class="pun">(</span><span class="pln">array</span><span class="pun">(</span><span class="str">'name'</span><span class="pun">,</span><span class="str">'email'</span><span class="pun">,</span><span class="str">'city'</span><span class="pun">));</span>
4.  <span class="com">// hide fields</span>
5.  <span class="pln">$app-></span><span class="pln">fields</span><span class="pun">(</span><span class="str">'name,email,city'</span><span class="pun">,</span> <span class="kwd">true</span><span class="pun">);</span>

You can create tabs for some fields, just set tab name in 3rd parameter:

1.  <span class="pln">$app-></span><span class="pln">fields</span><span class="pun">(</span><span class="str">'username,email'</span><span class="pun">,</span> <span class="kwd">false</span><span class="pun">,</span> <span class="str">'Account info'</span><span class="pun">);</span>
2.  <span class="pln">$app-></span><span class="pln">fields</span><span class="pun">(</span><span class="str">'country,city,sex,age'</span><span class="pun">,</span> <span class="kwd">false</span><span class="pun">,</span> <span class="str">'Personal'</span><span class="pun">);</span>

Also you can use different columns for different actions (**create, edit, view**), set action in 4th parameter:

1.  <span class="pln">$app-></span><span class="pln">fields</span><span class="pun">(</span><span class="str">'first_name,last_name,country'</span><span class="pun">,</span> <span class="kwd">false</span><span class="pun">,</span> <span class="kwd">false</span><span class="pun">,</span> <span class="str">'view'</span><span class="pun">);</span>

You can play with combination of this parameters. Tabs is unaviable if second parameter is **true**.

**show_primary_ai_field ()** - takes true / false, setting the display of primary auto-increment field when editing the entry.

1.  <span class="pln">$app-></span><span class="pln">show_primary_ai_field</span><span class="pun">(</span><span class="kwd">true</span><span class="pun">);</span>

Primary auto-increment field will be always disabled.

**readonly (** _field(s), mode_ **)** - Sets the selected fields attribute 'readonly'. Takes a string, separated by commas, or an array of values. Second paremeter sets mode (create, edit), default is all.

1.  <span class="pln">$app-></span><span class="kwd">readonly</span><span class="pun">(</span><span class="str">'name,email,city'</span><span class="pun">);</span>
2.  <span class="com">//or</span>
3.  <span class="pln">$app-></span><span class="kwd">readonly</span><span class="pun">(</span><span class="pln">array</span><span class="pun">(</span><span class="str">'name'</span><span class="pun">,</span><span class="str">'email'</span><span class="pun">,</span><span class="str">'city'</span><span class="pun">));</span>

**disabled (** _field(s), mode_ **)** - Sets the selected fields attribute 'disabled'. Takes a string, separated by commas, or an array of values. Second paremeter sets mode (create, edit), default is all.

1.  <span class="pln">$app-></span><span class="pln">disabled</span><span class="pun">(</span><span class="str">'name,email,city'</span><span class="pun">);</span>
2.  <span class="com">//or</span>
3.  <span class="pln">$app-></span><span class="pln">disabled</span><span class="pun">(</span><span class="pln">array</span><span class="pun">(</span><span class="str">'name'</span><span class="pun">,</span><span class="str">'email'</span><span class="pun">,</span><span class="str">'city'</span><span class="pun">));</span>
4.  <span class="pln"> </span>
5.  <span class="com">// separate screens</span>
6.  <span class="pln">$app-></span><span class="pln">disabled</span><span class="pun">(</span><span class="str">'email'</span><span class="pun">,</span><span class="str">'edit'</span><span class="pun">);</span>

**readonly_on_create (), readonly_on_edit (), disabled_on_create (), disabled_on_edit ()** - do the same thing as the methods above, but allow you to separate the creation and editing. This methods are deprecated and will be deleted.

**no_editor (** _field(s)_ **)** - allows you to load a text box without an editor (it has an effect when editor is loaded). Takes a string, separated by commas, or an array of values.

1.  <span class="pln">$app-></span><span class="pln">no_editor</span><span class="pun">(</span><span class="str">'name,email,city'</span><span class="pun">);</span>
2.  <span class="com">//or</span>
3.  <span class="pln">$app-></span><span class="pln">no_editor</span><span class="pun">(</span><span class="pln">array</span><span class="pun">(</span><span class="str">'name'</span><span class="pun">,</span><span class="str">'email'</span><span class="pun">,</span><span class="str">'city'</span><span class="pun">));</span>

**change_type (** _field(s), type, default, params_or_attr_ **)** - Allows you to change the visual representation of the field. Takes the name of the field, a new type, default value, and an additional parameter (a set of values for lists or the length of the field for the text boxes). Available field types: **bool, int, float, text, textarea, texteditor, date, datetime, timestamp, time, year, select, multiselect, password,<span style="text-decoration: line-through;"> _hidden_</span>, file, image, point.**

1.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'last_visit'</span><span class="pun">,</span><span class="str">'timestamp'</span><span class="pun">);</span>
2.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'nickname'</span><span class="pun">,</span><span class="str">'text'</span><span class="pun">,</span><span class="str">''</span><span class="pun">,</span><span class="lit">20</span><span class="pun">);</span>
3.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'email'</span><span class="pun">,</span><span class="str">'multiselect'</span><span class="pun">,</span><span class="str">'email2@ex.com'</span><span class="pun">,</span><span class="str">'email1@ex.com,email2@ex.com,email3@ex.com,email4@ex.com,email5@ex.com'</span><span class="pun">);</span>

See more about **change_type ()** in **Field types** section.

**Hidden** type will add hidden input in form. Use **fields()** and **pass_var()** metods to hide fields and put custom data in your table if you need to preserve your data (pass_var will not add any input).

**pass_var (** _field, value, mode_ **)** - Allows you to record a variable or data directly in the database, bypassing the add / edit form. The field may not be present in the field, edit field will not affect your data. Function takes the name of the field in the first parameter, and your data, or variable in the second. Optional third parameter is available that allows you to clearly define where you want to paste the data - when creating or editing.

1.  <span class="pln">$app-></span><span class="pln">pass_var</span><span class="pun">(</span><span class="str">'user_id'</span><span class="pun">,</span> <span class="pln">$user_id</span><span class="pun">);</span>
2.  <span class="pln">$app-></span><span class="pln">pass_var</span><span class="pun">(</span><span class="str">'created'</span><span class="pun">,</span> <span class="pln">date</span><span class="pun">(</span><span class="str">'Y-m-d H:i:s'</span><span class="pun">),</span> <span class="str">'create'</span><span class="pun">);</span>
3.  <span class="pln">$app-></span><span class="pln">pass_var</span><span class="pun">(</span><span class="str">'was_edited'</span><span class="pun">,</span> <span class="str">'Yes, it was!'</span><span class="pun">,</span> <span class="str">'edit'</span><span class="pun">);</span>
4.  <span class="pln"> </span>
5.  <span class="com">//using field from current row</span>
6.  <span class="pln">$app-></span><span class="pln">pass_var</span><span class="pun">(</span><span class="str">'modified'</span><span class="pun">,</span> <span class="str">'{last_action}'</span><span class="pun">);</span>

**pass_default(** _field, value_ **)** - pass default value into field, takes the name of the field and default  value, or array in first parameter.

1.  <span class="pln">$app-></span><span class="pln">pass_default</span><span class="pun">(</span><span class="str">'name'</span><span class="pun">,</span><span class="str">'Joe'</span><span class="pun">);</span>
2.  <span class="com">//or</span>
3.  <span class="pln">$app-></span><span class="pln">pass_default</span><span class="pun">(</span><span class="pln">array</span><span class="pun">(</span><span class="str">'name'</span> <span class="pun">=></span> <span class="str">'Joe'</span><span class="pun">,</span> <span class="str">'city'</span> <span class="pun">=></span> <span class="str">'Boston'</span><span class="pun">));</span>

**condition (** _field, operator, value, method, parameter(s)_ **)** - allows you to make some changes based on the data in the form. Takes field in first parameter, operator in second, value in 3rd, in the fourth - the method that is executed when triggered conditions, and in the fifth - the parameter passed to the method. Supported methods: **readonly(), readonly_on_create(), readonly_on_edit(), disabled(), disabled_on_create(), disabled_on_edit(), no_editor(), validation_required(), validation_pattern().** Supported operators: **=, >, <, >=, <=, !=, ^=, $=, ~=**.

1.  <span class="pln">$app-></span><span class="pln">condition</span><span class="pun">(</span><span class="str">'access'</span><span class="pun">,</span><span class="str">'<'</span><span class="pun">,</span><span class="str">'5'</span><span class="pun">,</span><span class="str">'disabled'</span><span class="pun">,</span><span class="str">'password,email,username'</span><span class="pun">);</span>
2.  <span class="com">// if access < 5 than make password, email and username not editable</span>
3.  <span class="com">// or</span>
4.  <span class="pln">$app-></span><span class="pln">condition</span><span class="pun">(</span><span class="str">'access'</span><span class="pun">,</span><span class="str">'<'</span><span class="pun">,</span><span class="str">'5'</span><span class="pun">,</span><span class="str">'validation_pattern'</span><span class="pun">,</span><span class="pln">array</span><span class="pun">(</span><span class="str">'username'</span><span class="pun">,</span><span class="str">'[0-9A-Za-z]+'</span><span class="pun">));</span>

**^=** - starts with, **$=** - ends with, **~=** - contains.

**validation_required (** _field(s), chars_ **)** - a simple rule that takes the name of the field and the number of characters (default - 1) or an array.

1.  <span class="pln">$app-></span><span class="pln">validation_required</span><span class="pun">(</span><span class="str">'name'</span><span class="pun">);</span>
2.  <span class="pln">$app-></span><span class="pln">validation_required</span><span class="pun">(</span><span class="str">'city'</span><span class="pun">,</span><span class="lit">3</span><span class="pun">);</span>
3.  <span class="com">//or</span>
4.  <span class="pln">$app-></span><span class="pln">validation_required</span><span class="pun">(</span><span class="pln">array</span><span class="pun">(</span><span class="str">'name'</span> <span class="pun">=></span> <span class="lit">1</span><span class="pun">,</span> <span class="str">'city'</span> <span class="pun">=></span> <span class="lit">3</span><span class="pun">));</span>
5.  <span class="pln"> </span>

**validation_pattern (** _field(s), pattren_ **)** - uses a simple validation pattern to selected fields. Takes the field name and the name of the pattern or array. Available patterns: **email, alpha, alpha_numeric, alpha_dash, numeric, integer, decimal, natural.** Also you can use your own regular expression in second parameter.

1.  <span class="pln">$app-></span><span class="pln">validation_pattern</span><span class="pun">(</span><span class="str">'name'</span><span class="pun">,</span> <span class="str">'alpha'</span><span class="pun">);</span>
2.  <span class="pln">$app-></span><span class="pln">validation_pattern</span><span class="pun">(</span><span class="str">'email'</span><span class="pun">,</span> <span class="str">'email'</span><span class="pun">);</span>
3.  <span class="com">//or</span>
4.  <span class="pln">$app-></span><span class="pln">validation_pattern</span><span class="pun">(</span><span class="pln">array</span><span class="pun">(</span><span class="str">'name'</span> <span class="pun">=></span> <span class="str">'alpha'</span><span class="pun">,</span> <span class="str">'email'</span> <span class="pun">=></span> <span class="str">'email'</span><span class="pun">));</span>
5.  <span class="com">// or</span>
6.  <span class="pln">$app-></span><span class="pln">validation_pattern</span><span class="pun">(</span><span class="str">'username'</span><span class="pun">,</span> <span class="str">'[a-zA-Z]{3,14}'</span><span class="pun">);</span>

**alert(** _email_field, cc, subject, body, link_ **), alert_edit(...), alert_creale(...)** - sends emails when data was changed: on create, update or both. Takes email from entry (you must define column in first parameter) and carbon copy list (if defined) from second parameter(can be comma-separated string or array). You must define subject in 3rd and email body in 4th parameter. You can define additional link for email in 5th parameter (optional).

In message body you can use simple tags to retrive some values from row, just write column name in {}.

1.  <span class="pln">$app-></span><span class="pln">alert</span><span class="pun">(</span><span class="str">'email'</span><span class="pun">,</span><span class="str">'admin@example.com'</span><span class="pun">,</span><span class="str">'Password changed'</span><span class="pun">,</span><span class="str">'Your new password is {password}'</span><span class="pun">);</span>
2.  <span class="pln">$app-></span><span class="pln">alert_create</span><span class="pun">(</span><span class="str">'email'</span><span class="pun">,</span><span class="str">''</span><span class="pun">,</span><span class="str">'Email Testing'</span><span class="pun">,</span><span class="str">'Hello!!!!!'</span><span class="pun">);</span>

**mass_alert(),** **mass_****alert_edit(),** **mass_****alert_create()** - the same like alert(), but it send message to all emails from selected table.

**mass_alert(**_email_table,email_column,email_where,subject,message,link,curent_param,param_value_**)**

1.  <span class="pln">$app-></span><span class="pln">mass_alert</span><span class="pun">(</span><span class="str">'user'</span><span class="pun">,</span><span class="str">'email'</span><span class="pun">,</span><span class="str">'subscribe = 1'</span><span class="pun">,</span><span class="str">'Email test'</span><span class="pun">,</span><span class="str">'Hello!'</span><span class="pun">);</span>

**page_call(**_url, data_array, param, param_value, method_**), page_call_create(...), page_call_edit(...)** - sends http query to another file/page. This method not receive any data. It only sends request when row was added or changed.

You can define any data into **data_array**, including {field_tags}, this data will be received in target page like $_GET or $_POST array. If **param** is defined, method will run only if field value from current row (**param**) will be equal to **param_value**_._ **Method** defines type of request (get or post), default is get.

1.  <span class="pln">$app-></span><span class="pln">page_call</span><span class="pun">(</span><span class="str">'http://mysite/admin/mytarget.php'</span><span class="pun">,</span> <span class="pln">array</span><span class="pun">(</span><span class="str">'id'</span><span class="pun">=></span><span class="str">'{user_id}'</span><span class="pun">,</span><span class="str">'text'</span><span class="pun">=></span><span class="str">'User {user_name} here!'</span><span class="pun">));</span>

<div class="alert">If your site using authorization, you can try to send browser cookie with request. This featue is experimental and can be turn on from configuration file. **BE CAREFUL: DON'T USE IT FOR REQUESTS TO EXTERNAL SITES!!!**</div>

**set_attr(** _field(s), array attr_ **)** - allows to set custom attributes for fields in edit form. _field(s)_ - form field(s), _attr_ - assoc array with attributes.

1.  <span class="pln">$app-></span><span class="pln">set_attr</span><span class="pun">(</span><span class="str">'user'</span><span class="pun">,</span><span class="pln">array</span><span class="pun">(</span><span class="str">'id'</span><span class="pun">=></span><span class="str">'user'</span><span class="pun">,</span><span class="str">'data-role'</span><span class="pun">=></span><span class="str">'admin'</span><span class="pun">));</span>

</section>

<section id="field_types">

<div class="page-header">

### 4\. Field types

* * *

</div>

Function **change_type ()** a little difficult to understand, because it uses a specific syntax, which varies depending on the task. Let's try to beat these tasks for conventional groups.

The first three parameters are always the same. They take the name of the field, the desired type of field and default value. The third and the fourth parameter is not required. If you do not specify them, will be loaded values by default.

4th parameter - array with type-parameters and/or html attributes. You can use custom html attributes in most of types.

#### 1\. Text fields.

This group includes the following types of fields: **text, int, float, <span style="text-decoration: line-through;">_hidden_</span>.**

For these types the function of the default value in the third parameter, and the maximum number of characters in the fourth parameter (exclude <span style="text-decoration: line-through;">_**hidden**_</span>).

1.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'user_code'</span><span class="pun">,</span> <span class="str">'int'</span><span class="pun">,</span> <span class="str">'000'</span><span class="pun">,</span> <span class="lit">10</span><span class="pun">);</span> <span class="com">// v1.5 legacy</span>
2.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'user_code'</span><span class="pun">,</span> <span class="str">'int'</span><span class="pun">,</span> <span class="str">'000'</span><span class="pun">,</span> <span class="pln">array</span><span class="pun">(</span><span class="str">'maxlength'</span><span class="pun">=></span><span class="lit">10</span><span class="pun">));</span> <span class="com">// v1.6</span>

Use **fields()** and **pass_var()** metods to hide fields and put custom data in your table.

#### 2\. Multi-line text fields.

This group includes the following types of fields: **textarea,** **texteditor****.**

This types no need any additional parameters.

1.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'user_desc'</span><span class="pun">,</span> <span class="str">'textarea'</span><span class="pun">);</span>

#### 3\. Date fields.

This group includes the following types of fields: **date, datetime, time, year, timestamp.**

These types of fields will only accept the default value in the third parameter.

1.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'created'</span><span class="pun">,</span> <span class="str">'datetime'</span><span class="pun">,</span> <span class="str">'2012-10-22 07:01:33'</span><span class="pun">);</span>

Date range can be used with **date** type:

1.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'created'</span><span class="pun">,</span> <span class="str">'date'</span><span class="pun">,</span> <span class="str">''</span><span class="pun">,</span> <span class="pln">array</span><span class="pun">(</span><span class="str">'range_end'</span><span class="pun">=></span><span class="str">'end_date'</span><span class="pun">));</span> <span class="com">// this is start date field and it points to end date field</span>
2.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'created'</span><span class="pun">,</span> <span class="str">'date'</span><span class="pun">,</span> <span class="str">''</span><span class="pun">,</span> <span class="pln">array</span><span class="pun">(</span><span class="str">'range_start'</span><span class="pun">=></span><span class="str">'start_date'</span><span class="pun">));</span> <span class="com">// this is end of range date and it points to the start date range date</span>

This will create relation between two date fields.

#### 4\. Lists.

This group includes the following types of fields: **select, multiselect, radio, checkboxes**

These types of fields are set to default in the third argument, and a list of available values, separated by commas, in the second parameter. **multiselect** can contain several values by default.

1.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'adm_email'</span><span class="pun">,</span><span class="str">'select'</span><span class="pun">,</span><span class="str">'email2@ex.com'</span><span class="pun">,</span><span class="str">'email1@ex.com,email2@ex.com,email3@ex.com,email4@ex.com,email5@ex.com'</span><span class="pun">);</span>
2.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'fav_color'</span><span class="pun">,</span><span class="str">'multiselect'</span><span class="pun">,</span><span class="str">'black,white'</span><span class="pun">,</span><span class="str">'red,blue,yellow,green,black,white'</span><span class="pun">);</span>
3.  <span class="pln"> </span>
4.  <span class="com">// v1.6 new syntax</span>
5.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'fav_color'</span><span class="pun">,</span><span class="str">'multiselect'</span><span class="pun">,</span><span class="str">'black,white'</span><span class="pun">,</span> <span class="pln">array</span><span class="pun">(</span><span class="str">'values'</span><span class="pun">=></span><span class="str">'red,blue,yellow,green,black,white'</span><span class="pun">));</span>

You  can show optgroups in dropdowns. Just us multidimensional array:

1.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'country'</span><span class="pun">,</span><span class="str">'select'</span><span class="pun">,</span><span class="str">''</span><span class="pun">,</span><span class="pln">array</span><span class="pun">(</span><span class="str">'values'</span><span class="pun">=></span><span class="pln">array</span><span class="pun">(</span><span class="str">'Europe'</span><span class="pun">=></span><span class="pln">array</span><span class="pun">(</span><span class="str">'UK'</span><span class="pun">=></span><span class="str">'United Kindom'</span><span class="pun">,</span><span class="str">'FR'</span><span class="pun">=></span><span class="str">'France'</span><span class="pun">),</span><span class="str">'Asia'</span><span class="pun">=></span><span class="pln">array</span><span class="pun">(</span><span class="str">'RU'</span><span class="pun">=></span><span class="str">'Russia'</span><span class="pun">,</span><span class="str">'CH'</span><span class="pun">=></span><span class="str">'China'</span><span class="pun">))));</span>

#### 5\. Password.

This group includes the following types of fields: **password.**

Takes the type of hashing in the third parameter (e.g. **md5** or **sha1** or **sha256...** , leave blank if you do not want to use hashing) and the maximum number of characters in the fourth. The peculiarity of this field is that it does not load the values and does not store the null value (not allowing you to change the previous one to null). This is due to the fact that passwords are usually encrypted or hashed before being stored in the database.

1.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'user_key'</span><span class="pun">,</span> <span class="str">'password'</span><span class="pun">,</span> <span class="str">''</span><span class="pun">,</span> <span class="lit">32</span><span class="pun">);</span>
2.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'user_pass'</span><span class="pun">,</span> <span class="str">'password'</span><span class="pun">,</span> <span class="str">'sha256'</span><span class="pun">,</span> <span class="lit">8</span><span class="pun">);</span>
3.  <span class="com">// attributes example</span>
4.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'user_pass'</span><span class="pun">,</span> <span class="str">'password'</span><span class="pun">,</span> <span class="str">'md5'</span><span class="pun">,</span> <span class="pln">array</span><span class="pun">(</span><span class="str">'maxlength'</span><span class="pun">=></span><span class="lit">10</span><span class="pun">,</span><span class="str">'placeholder'</span><span class="pun">=></span><span class="str">'enter password'</span><span class="pun">));</span>

#### 6\. Upload.

This group includes the following types of fields: **file, image.**

Takes the path to the upload folder (must exist and be writable) in the third parameter and configuration array in the fourth.

For all uploaded files magic creates a unique name to avoid overwriting, but you can cancel the renaming. In this case, the file name will be brought to the alpha-numeric pattern.

1.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'attach'</span><span class="pun">,</span> <span class="str">'file'</span><span class="pun">,</span> <span class="str">''</span><span class="pun">,</span> <span class="pln">array</span><span class="pun">(</span><span class="str">'not_rename'</span><span class="pun">=></span><span class="kwd">true</span><span class="pun">));</span>

You can resize the uploaded **image**, and crop them at different proportions.

1.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'photo'</span><span class="pun">,</span><span class="str">'image'</span><span class="pun">,</span><span class="str">''</span><span class="pun">,</span><span class="pln">array</span><span class="pun">(</span><span class="str">'width'</span><span class="pun">=></span><span class="lit">300</span><span class="pun">));</span> <span class="com">// resize main image</span>
2.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'photo'</span><span class="pun">,</span><span class="str">'image'</span><span class="pun">,</span><span class="str">''</span><span class="pun">,</span><span class="pln">array</span><span class="pun">(</span><span class="str">'width'</span><span class="pun">=></span><span class="lit">300</span><span class="pun">,</span> <span class="str">'height'</span><span class="pun">=></span><span class="lit">300</span><span class="pun">,</span> <span class="str">'crop'</span><span class="pun">=></span><span class="kwd">true</span><span class="pun">));</span> <span class="com">// auto-crop</span>
3.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'photo'</span><span class="pun">,</span><span class="str">'image'</span><span class="pun">,</span><span class="str">''</span><span class="pun">,</span><span class="pln">array</span><span class="pun">(</span><span class="str">'manual_crop'</span><span class="pun">=></span><span class="kwd">true</span><span class="pun">));</span> <span class="com">// crop it as you want</span>

You can create a preview picture for your image to be stored in that same folder, and a marker will differ at the end of the name.  You can also specify the size of thumbnails, resize method and subfolder.

<div class="alert">Note: 'Manual crop' will crop thumbnails in same proportion as main image</div>

You can add any number of thumbs, you must create subarray in thumbs array  for each thumbnail

1.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'photo'</span><span class="pun">,</span><span class="str">'image'</span><span class="pun">,</span><span class="str">''</span><span class="pun">,</span><span class="pln">array</span><span class="pun">(</span>
2.  <span class="str">'thumbs'</span><span class="pun">=></span><span class="pln">array</span><span class="pun">(</span>
3.  <span class="pln">array</span><span class="pun">(</span><span class="str">'width'</span><span class="pun">=></span> <span class="lit">50</span><span class="pun">,</span> <span class="str">'marker'</span><span class="pun">=></span><span class="str">'_small'</span><span class="pun">),</span>
4.  <span class="pln">array</span><span class="pun">(</span><span class="str">'width'</span><span class="pun">=></span> <span class="lit">100</span><span class="pun">,</span> <span class="str">'marker'</span><span class="pun">=></span><span class="str">'_middle'</span><span class="pun">),</span>
5.  <span class="pln">array</span><span class="pun">(</span><span class="str">'width'</span> <span class="pun">=></span> <span class="lit">150</span><span class="pun">,</span> <span class="str">'folder'</span> <span class="pun">=></span> <span class="str">'thumbs'</span> <span class="com">// using 'thumbs' subfolder</span>
6.  <span class="pun">)</span>
7.  <span class="pun">));</span>

You can save your image as a binary string in the database. For this field in your database must be of type BLOB, MEDIUMBLOB or LONGBLOB. Thumbnail creation in this case is not available. If you do not know how to extract data from these fields, it is better use the normal load.

1.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'photo'</span><span class="pun">,</span><span class="str">'image'</span><span class="pun">,</span><span class="str">''</span><span class="pun">,</span><span class="pln">array</span><span class="pun">(</span><span class="str">'blob'</span><span class="pun">=></span><span class="kwd">true</span><span class="pun">));</span>

**Uploads parameters:**

Files:

*   **not_rename** - _(true/false)_ - disables auto-renaming of uploaded files
*   **text** - _(string)_ - display custom file name
*   **path** - relative or absolute path to uploads folder
*   **blob** - _(true/false)_ - saves image as binary string in database blob field
*   **filename** - _(string)_ - name of downloadable file
*   **url** - _(string)_ -real url to upload folder (optional, if you want to use real links)

Images:

*   **not_rename** - _(true/false)_ - disables auto-renaming of uploaded files
*   **path** - relative or absolute path to uploads folder
*   **width, height** - _(integer)_ - sets dimensions for image, if both is not set image will not been resized
*   **crop** - _(true/false)_ - image will be cropped (if not set, image will be saved with saving proportions). Both width and height required
*   **manual_crop** - _(__true/false__)_ -  Allows to crop image manually
*   **ratio** - _(float)_ - cropped area ratio (uses with manual_crop)
*   **watermark** - _(string)_ - relative or absolute path to watermark image
*   **watermark_position** - _(array)_ - array with two elements (left,top), sets watermark offsets in persents (example: array(95,5) - right top corner)
*   **blob** - _(true/false)_ - saves image as binary string in database blob field. Thumbnails creation is not available.
*   **grid_thumb** - (_int_) - number of thumb, which will be displayed in grid. If not set, original image will be displayed.
*   **detail_thumb** - (_int_) - number of thumb, which will be displayed in detail view/create/edit . If not set, original image will be displayed.
*   **url** - _(string)_ -real url to upload folder (optional, if you want to use real links)
*   **thumbs** - array of thumb arrays**:**
    *   **width, height,** **crop,** **watermark,** **watermark_position** - see parent
    *   **marker -** _(string)_ - thumbnail marker (if you not set marker or folder, the main image will be replaced with thumbnail)
    *   **folder -** _(string)_ - thumbnail subfolder, relative to uploads folder (if you not set marker or folder, the main image will be replaced with thumbnail)

<div class="alert">All paths are always relative to magic's folder</div>

#### 7\. Price

This group includes the following types of fields: **price.**

Takes default value in third parameter and array of parameters in 4th.

1.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'amount'</span><span class="pun">,</span> <span class="str">'price'</span><span class="pun">,</span> <span class="str">'5'</span><span class="pun">,</span> <span class="pln">array</span><span class="pun">(</span><span class="str">'prefix'</span><span class="pun">=></span><span class="str">'/span></span><span class="pun">));</span>

**Parameters:**

*   **max** - _(integer)_ - maximal length of text field (create/update), default is 10
*   **decimals** - _(integer)_ - cout of decimals, default is 2
*   **separator** - _(string)_ - thousands separator, default is comma (list view)
*   **prefix** - _(string)_ - prefix for list view
*   **suffix** - _(string)_ - suffix for list view
*   **point** - _(string)_ - decimal point for list view

#### 8\. Remote images

This group includes the following types of fields: **remote_image.**

Takes default value in 3rd parameter and link part in 4th (e.g. if your field contains only file name, not full url).

1.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'avatar'</span><span class="pun">,</span> <span class="str">'remote_image'</span><span class="pun">);</span>
2.  <span class="com">// or</span>
3.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'avatar'</span><span class="pun">,</span> <span class="str">'remote_image'</span><span class="pun">,</span> <span class="str">''</span><span class="pun">,</span> <span class="str">'http://my-img-host.net/my-folder/'</span><span class="pun">);</span>
4.  <span class="com">// 1.6</span>
5.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'avatar'</span><span class="pun">,</span> <span class="str">'remote_image'</span><span class="pun">,</span> <span class="str">''</span><span class="pun">,</span> <span class="pln">array</span><span class="pun">(</span><span class="str">'link'</span><span class="pun">=></span><span class="str">'http://my-img-host.net/my-folder/'</span><span class="pun">));</span>

#### 9\. Locations and maps

This group includes the following types of fields: **point.**

This automatically works with 'POINT' mysql field type. In text field type coordinates will be saved as string separated with comma. 4th parameter gets array with map settings, also you can define them in configuration file to use by default.

1.  <span class="pln">$app-></span><span class="pln">change_type</span><span class="pun">(</span><span class="str">'my_location'</span><span class="pun">,</span><span class="str">'point'</span><span class="pun">,</span><span class="str">'39.909736,-6.679687'</span><span class="pun">,</span><span class="pln">array</span><span class="pun">(</span><span class="str">'text'</span><span class="pun">=></span><span class="str">'Your are here'</span><span class="pun">));</span>

**Parameters:**

*   **width** - _(integer)_ - width of map
*   **height** - _(integer)_ - height of map
*   **zoom** - _(integer)_ - map zoom
*   **text** - _(string)_ - text in info window
*   **search_text -** _(__string__)_ - placeholder for search field
*   **search** - _(true/false)_ - show search field
*   **coords** - _(true/false)_ - show coordinates field

</section>

<section id="0_data_manipulation">

<div class="page-header">

### 5\. Data manipulation

* * *

</div>

magic use loadable libraries for data manipulation. You can use static functions in the classes, methods, objects, or simply a set of procedural functions. You need to specify the path to your library functions, and magic it connects itself, when in fact will need. magic will call own functions.php file when path is not defined. magic instance will be always available in your function as last parameter.

<div class="alert alert-info">**callable** - is callable parameter, if you want to call method from class, you must use an array, e.g. array('class', 'method'). For procedural function just set function name.</div>

**$postdata** - the object, which represented magic's form data sent to server side. It has three methods:

*   **$postdata->get(** _field_ **)** - returns value of a field or FALSE if field is not exist
*   **$postdata->set(** _field, value_ **)** - sets new value in a field or creates new field in $postdata. Returns nothing. You no need to use unlock_field() method anymore.
*   **$postdata->to_array()** - returns all data in array.
*   **$postdata->del(** _field_ **)** - removes field from dataset. Be careful, this can make errors in application.

**before_insert (**_callable, path_**)** - allows you to prepare the data before inserting it into the database. Takes an callable in first parameter and the library path in second.  Your function should take postdata object in the first parameter, magic's instance in second and no need to return anything.

1.  <span class="com">// function in functions.php</span>
2.  <span class="kwd">function</span> <span class="pln">hash_password</span><span class="pun">(</span><span class="pln">$postdata</span><span class="pun">,</span> <span class="pln">$app</span><span class="pun">){</span>
3.  <span class="pln">$postdata</span><span class="pun">-></span><span class="kwd">set</span><span class="pun">(</span><span class="str">'password'</span><span class="pun">,</span> <span class="pln">sha1</span><span class="pun">(</span> <span class="pln">$postdata</span><span class="pun">-></span><span class="kwd">get</span><span class="pun">(</span><span class="str">'password'</span><span class="pun">)</span> <span class="pun">));</span>
4.  <span class="pun">}</span>

1.  <span class="com">// creating action</span>
2.  <span class="pln">$app-></span><span class="pln">before_insert</span><span class="pun">(</span><span class="str">'hash_password'</span><span class="pun">);</span> <span class="com">// automatic call of functions.php</span>
3.  <span class="com">//or</span>
4.  <span class="pln">$app-></span><span class="pln">before_insert</span><span class="pun">(</span><span class="str">'hash_password'</span><span class="pun">,</span> <span class="str">'functions.php'</span><span class="pun">);</span> <span class="com">// manualy load</span><span id="cke_bm_109E" style="display: none;"></span>

**before_update (**_callable, path_**)** - allows you to prepare the data before updating the records in the database. Takes a callable as the first parameter and the library path in second. Your function should take an array of data in the first parameter and the same return. As well as optionally the primary key in the second parameter.

1.  <span class="com">// function in functions.php</span>
2.  <span class="kwd">function</span> <span class="pln">hash_password</span><span class="pun">(</span><span class="pln">$postdata</span><span class="pun">,</span> <span class="pln">$primary</span><span class="pun">,</span> <span class="pln">$app</span><span class="pun">){</span>
3.  <span class="pln">$postdata</span><span class="pun">-></span><span class="kwd">set</span><span class="pun">(</span><span class="str">'password'</span><span class="pun">,</span> <span class="pln">sha1</span><span class="pun">(</span> <span class="pln">$postdata</span><span class="pun">-></span><span class="kwd">get</span><span class="pun">(</span><span class="str">'password'</span><span class="pun">)</span> <span class="pun">));</span>
4.  <span class="pun">}</span>

1.  <span class="com">// creating action</span>
2.  <span class="pln">$app-></span><span class="pln">before_update</span><span class="pun">(</span><span class="str">'hash_password'</span><span class="pun">);</span>

**after_insert (**_callable, path_**), after_update (**_callable, path_**)** - allow you to pass data and primary key after upgrading to the custom function. The syntax is the same as in **before_update (),** except that the function is not obliged to return something.

**before_remove (**_callable, path_**), after_remove (**_callable, path_**)** - sends to user-defined function only the primary key record to be deleted in the first parameter. The syntax is the same as in previous examples. User-defined function is not obliged to return something.

1.  <span class="com">// function in functions.php</span>
2.  <span class="kwd">function</span> <span class="pln">delete_user_data</span><span class="pun">(</span><span class="pln">$primary</span><span class="pun">,</span> <span class="pln">$app</span><span class="pun">){</span>
3.  <span class="pln">$db</span> <span class="pun">=</span> <span class="typ">magic_db</span><span class="pun">::</span><span class="pln">get_instance</span><span class="pun">();</span>
4.  <span class="pln">$db</span><span class="pun">-></span><span class="pln">query</span><span class="pun">(</span><span class="str">'DELETE FROM gallery WHERE user_id = '</span> <span class="pun">.</span> <span class="pln">$db</span><span class="pun">-></span><span class="pln">escape</span><span class="pun">(</span><span class="pln">$primary</span><span class="pun">));</span>
5.  <span class="pun">}</span>

1.  <span class="com">// creating action</span>
2.  <span class="pln">$app-></span><span class="pln">before_remove</span><span class="pun">(</span><span class="str">'delete_user_data'</span><span class="pun">);</span>

**column_callback (**_column, callable,_ _path_**)** - allows you to define custom layer for your column data.

1.  <span class="pln">$app-></span><span class="pln">column_callback</span><span class="pun">(</span><span class="str">'name'</span><span class="pun">,</span><span class="str">'add_user_icon'</span><span class="pun">);</span>

functions.php:

1.  <span class="pun"><?</span><span class="pln">php</span>
2.  <span class="kwd">function</span> <span class="pln">add_user_icon</span><span class="pun">(</span><span class="pln">$value</span><span class="pun">,</span> <span class="pln">$fieldname</span><span class="pun">,</span> <span class="pln">$primary_key</span><span class="pun">,</span> <span class="pln">$row</span><span class="pun">,</span> <span class="pln">$app</span><span class="pun">)</span>
3.  <span class="pun">{</span>
4.  <span class="kwd">return</span> <span class="str">'<i class="icon-user"></i>'</span> <span class="pun">.</span> <span class="pln">$value</span><span class="pun">;</span>
5.  <span class="pun">}</span>
6.  <span class="pun">?></span>

_**$row** - full current row._

**field_callback (**_field,_ _callable,_ _path_**)** - allows you to define custom layer for your edit field.

1.  <span class="pln">$app-></span><span class="pln">field_callback</span><span class="pun">(</span><span class="str">'name'</span><span class="pun">,</span><span class="str">'nice_input'</span><span class="pun">);</span>

functions.php:

1.  <span class="pun"><?</span><span class="pln">php</span>
2.  <span class="kwd">function</span> <span class="pln">nice_input</span><span class="pun">(</span><span class="pln">$value</span><span class="pun">,</span> <span class="pln">$field</span><span class="pun">,</span> <span class="pln">$priimary_key</span><span class="pun">,</span> <span class="pln">$list</span><span class="pun">,</span> <span class="pln">$app</span><span class="pun">)</span>
3.  <span class="pun">{</span>
4.  <span class="kwd">return</span> <span class="str">'<div class="input-prepend input-append">'</span>
5.  <span class="pun">.</span> <span class="str">'<span class="add-on">$</span>'</span>
6.  <span class="pun">.</span> <span class="str">'<input type="text" name="'</span><span class="pun">.</span><span class="pln">$app-></span><span class="pln">fieldname_encode</span><span class="pun">(</span><span class="pln">$fieldname</span><span class="pun">).</span><span class="str">'" value="'</span><span class="pun">.</span><span class="pln">$value</span><span class="pun">.</span><span class="str">'" class="magic-input" />'</span>
7.  <span class="pun">.</span> <span class="str">'<span class="add-on">.00</span>'</span>
8.  <span class="pln">   </span> <span class="pun">.</span> <span class="str">'</div>'</span><span class="pun">;</span>
9.  <span class="pun">}</span>
10.  <span class="pun">?></span>

_**$list** - all fields data_

**_fieldname_encode_()** - this encodes field name in compatible format.

You can use **data-required** and **data-pattern** attributes, and **unique** class.

Important: you need to use **magic-input** class for your custom inputs to make its usable by magic.

**replace_insert(c**_allable, path_**), replase_update(...), replace_remove(...)** - replaces standsrt magic actions (insert, update, remove) by custom function. Each of this methods passes in the the target function its parameters.

Target function for **replace_remove()**

1.  <span class="kwd">function</span> <span class="pln">remove_replacer</span><span class="pun">(</span><span class="pln">$primary_key</span><span class="pun">,</span> <span class="pln">$app</span><span class="pun">){</span> <span class="pun">...</span> <span class="pun">}</span>

Target function for **replace_insert()**

1.  <span class="kwd">function</span> <span class="pln">insert_replacer</span><span class="pun">(</span><span class="pln">$postdata</span><span class="pun">,</span> <span class="pln">$app</span><span class="pun">){</span> <span class="pun">...</span> <span class="pun">}</span>

Target function for **replace_update()**

1.  <span class="kwd">function</span> <span class="pln">update_replacer</span><span class="pun">(</span><span class="pln">$postdata</span><span class="pun">,</span> <span class="pln">$primary_key</span><span class="pun">,</span> <span class="pln">$app</span><span class="pun">){</span> <span class="pun">...</span> <span class="pun">}</span>

_$app - magic current instance object._

<div class="alert alert-error">**replace_insert()** and **replace_update()** methods must return value of primary key!</div>

**call_update(** _postdata, primary_key_ **)**

You can call magic update method in your replace_insert(), replase_update(), replace_remove() target functions:

1.  <span class="pln">$app-></span><span class="pln">call_update</span><span class="pun">(</span><span class="pln">$postdata</span><span class="pun">,</span> <span class="pln">$primary_key</span><span class="pun">);</span>

#### Upload callbacks

**before_upload(**c_allable, path_**)** - runs when files was sent to a server, but not processed yet.

**after_upload(**c_allable, path_**)** - file was uploaded and moved to deinstanation folder (but not resized yet, if image).

**after_resize(**c_allable, path_**)** - file was already resized.  Available only for images.

Callback function example:

1.  <span class="kwd">function</span> <span class="pln">file_callback</span><span class="pun">(</span><span class="pln">$field</span><span class="pun">,</span><span class="pln">$filename</span><span class="pun">,</span><span class="pln">$file_path</span><span class="pun">,</span><span class="pln">$config</span><span class="pun">,</span><span class="pln">$app</span><span class="pun">){</span>
2.  <span class="pun">...</span>
3.  <span class="pun">}</span>

*   **$field** - full field name (table and culumn with dot separator)
*   **$filename** - name of the file (not available for **before_upload**())
*   **$file_path** - full file path with file name (not available for **before_upload**())
*   **$config** - array with parameters from change_type() method (4th parameter).
*   **$app** - magic instance

#### Other callbacks

**before_list(**c_allable, path_**)** - can run before grid will be displayed.

1.  <span class="com">// function in functions.php</span>
2.  <span class="kwd">function</span> <span class="pln">before_list_callback</span><span class="pun">(</span><span class="pln">$grid_data</span><span class="pun">,</span> <span class="pln">$app</span><span class="pun">){</span>
3.  <span class="pln">print_r</span><span class="pun">(</span><span class="pln">$grid_data</span><span class="pun">);</span>
4.  <span class="pun">}</span>

**before_create(**c_allable, path_**)** , **before_edit**(...), **before_view**(...) - can run before details view will be displayed.

1.  <span class="com">// function in functions.php (before edit/view)</span>
2.  <span class="kwd">function</span> <span class="pln">before_details_callback</span><span class="pun">(</span><span class="pln">$row_data</span><span class="pun">,</span> <span class="pln">$primary</span><span class="pun">,</span> <span class="pln">$app</span><span class="pun">){</span>
3.  <span class="pln">echo $row_data</span><span class="pun">-></span><span class="kwd">get</span><span class="pun">(</span><span class="str">'username'</span><span class="pun">);</span> <span class="com">// like 'postdata'</span>
4.  <span class="pun">}</span>

1.  <span class="com">// function in functions.php (before create, there is no primary)</span>
2.  <span class="kwd">function</span> <span class="pln">before_details_callback</span><span class="pun">(</span><span class="pln">$row_data</span><span class="pun">,</span> <span class="pln">$app</span><span class="pun">){</span>
3.  <span class="pln">echo $row_data</span><span class="pun">-></span><span class="kwd">get</span><span class="pun">(</span><span class="str">'username'</span><span class="pun">);</span> <span class="com">// like 'postdata'</span>
4.  <span class="pun">}</span>

#### Database instanse

In all external files you can use magic **database instanse**:

1.  <span class="pln">$db</span> <span class="pun">=</span> <span class="typ">magic_db</span><span class="pun">::</span><span class="pln">get_instance</span><span class="pun">();</span>
2.  <span class="pln">$db</span><span class="pun">-></span><span class="pln">query</span><span class="pun">(...)</span> <span class="com">// executes query, returns count of affected rows</span>
3.  <span class="pln">$db</span><span class="pun">-></span><span class="pln">result</span><span class="pun">();</span> <span class="com">// loads results as list of arrays</span>
4.  <span class="pln">$db</span><span class="pun">-></span><span class="pln">row</span><span class="pun">();</span> <span class="com">// loads one result row as associative array</span>
5.  <span class="pln">$var</span> <span class="pun">=</span> <span class="pln">$db</span><span class="pun">-></span><span class="pln">escape</span><span class="pun">(</span><span class="pln">$var</span><span class="pun">);</span> <span class="com">// escapes variable, ads single quotes</span>
6.  <span class="pln">$db</span><span class="pun">-></span><span class="pln">insert_id</span><span class="pun">();</span> <span class="com">// returns last insert id</span>

#### Control file type with upload callback (quick sample):

1.  <span class="pln">$app-></span><span class="pln">after_upload</span><span class="pun">(</span><span class="str">'after_upload_example'</span><span class="pun">);</span>

functions.php:

1.  <span class="kwd">function</span> <span class="pln">after_upload_example</span><span class="pun">(</span><span class="pln">$field</span><span class="pun">,</span> <span class="pln">$file_name</span><span class="pun">,</span> <span class="pln">$file_path</span><span class="pun">,</span> <span class="pln">$params</span><span class="pun">,</span> <span class="pln">$app</span><span class="pun">){</span>
2.  <span class="pln">$ext</span> <span class="pun">=</span> <span class="pln">trim</span><span class="pun">(</span><span class="pln">strtolower</span><span class="pun">(</span><span class="pln">strrchr</span><span class="pun">(</span><span class="pln">$file_name</span><span class="pun">,</span> <span class="str">'.'</span><span class="pun">)),</span> <span class="str">'.'</span><span class="pun">);</span>
3.  <span class="kwd">if</span><span class="pun">(</span><span class="pln">$ext</span> <span class="pun">!=</span> <span class="str">'pdf'</span> <span class="pun">&&</span> <span class="pln">$field</span> <span class="pun">==</span> <span class="str">'uploads.simple_upload'</span><span class="pun">){</span> <span class="com">// you can use other check-in</span>
4.  <span class="pln">unlink</span><span class="pun">(</span><span class="pln">$file_path</span><span class="pun">);</span>
5.  <span class="pln">$app-></span><span class="pln">set_exception</span><span class="pun">(</span><span class="str">'simple_upload'</span><span class="pun">,</span><span class="str">'This is not PDF'</span><span class="pun">,</span><span class="str">'error'</span><span class="pun">);</span>
6.  <span class="pun">}</span>
7.  <span class="pun">}</span>

#### Additional variables

This methods allow you to send custom data in magic's templates or in calback functions.

**set_var(** _var_name, value_ **)** - set user variable

**get_var(** _var_name_ **)** - get user variable

**unset_var(** _var_name_ **)** - unset user variable

#### Interactive ajax callbacks

You can easy create some actions with callback on server side in magic's grid

First you must create callback (like other callbacks) with method 'create action()'.

**create_action(** _action_name, callable, [path]_ **)** - _action_name_ - name of your action, _callable_ - name of your callback (function or method), _path (optional)_ - path to your file with function, default is 'functions.php'.

1.  <span class="pln">$app-></span><span class="pln">create_action</span><span class="pun">(</span><span class="str">'my_action'</span><span class="pun">,</span><span class="str">'my_function'</span><span class="pun">);</span>

Than, create function in your functions.php (or you can use other file)

1.  <span class="kwd">function</span> <span class="pln">my_functions</span><span class="pun">(</span><span class="pln">$app</span><span class="pun">){</span>
2.  <span class="com">// some manipulations</span>
3.  <span class="pun">}</span>

Finally, create button or link (or any) with specific class and data attributes. Simpliest button will be:

1.  <span class="tag"><button</span> <span class="atn">class</span><span class="pun">=</span><span class="atv">"magic-action"</span> <span class="atn">data-task</span><span class="pun">=</span><span class="atv">"action"</span> <span class="atn">data-action</span><span class="pun">=</span><span class="atv">"my_action"</span><span class="tag">></span><span class="pln">GO!</span><span class="tag"></button></span>

This is required. You can define any other data attributes and all of them will be sent with action. To receive them in callback function you must call **get()** method with your attribute name in parameter (without data- prefix):

1.  <span class="kwd">function</span> <span class="pln">my_functions</span><span class="pun">(</span><span class="pln">$app</span><span class="pun">){</span>
2.  <span class="pln">$myvalue</span> <span class="pun">=</span> <span class="pln">$app-></span><span class="kwd">get</span><span class="pun">(</span><span class="str">'myvalue'</span><span class="pun">);</span> <span class="com">// data-myvalue attr.</span>
3.  <span class="com">// some manipulations</span>
4.  <span class="pun">}</span>

#### Returning errors from callback function

You can do you own validation or something more in any callback function during saving and return user back to form.

**set_exception(** _[fields] , [text], [message_type]_ **)** - fields - highlighted fields in form, text - error message, message_type - type of error message (error, note, success, info), just css class for message box, default is 'error'.  
 All arguments is optional.

1.  <span class="com">// function in functions.php</span>
2.  <span class="kwd">function</span> <span class="pln">hash_password</span><span class="pun">(</span><span class="pln">$postdata</span><span class="pun">,</span> <span class="pln">$primary</span><span class="pun">,</span> <span class="pln">$app</span><span class="pun">){</span>
3.  <span class="pln">$pass</span> <span class="pun">=</span> <span class="pln">$postdata</span><span class="pun">-></span><span class="kwd">get</span><span class="pun">(</span><span class="str">'password'</span><span class="pun">);</span>
4.  <span class="kwd">if</span><span class="pun">(</span><span class="pln">preg_match</span><span class="pun">(</span><span class="str">'/[a-zA-Z]+/u'</span><span class="pun">,</span><span class="pln">$pass</span><span class="pun">)</span> <span class="pun">&&</span> <span class="pln">preg_match</span><span class="pun">(</span><span class="str">'/[0-9]+/u'</span><span class="pun">,</span><span class="pln">$pass</span><span class="pun">)){</span>
5.  <span class="pln">$postdata</span><span class="pun">-></span><span class="kwd">set</span><span class="pun">(</span><span class="str">'password'</span><span class="pun">,</span> <span class="pln">sha1</span><span class="pun">(</span> <span class="pln">$pass</span> <span class="pun">)</span> <span class="pun">);</span>
6.  <span class="pun">}</span>
7.  <span class="kwd">else</span><span class="pun">{</span>
8.  <span class="pln">$app-></span><span class="pln">set_exception</span><span class="pun">(</span><span class="str">'password'</span><span class="pun">,</span><span class="str">'Your password is too simple.'</span><span class="pun">);</span>
9.  <span class="pun">}</span>
10.  <span class="pun">}</span>

</section>

<section id="1_themes_guide">

<div class="page-header">

### 6\. Themes guide

* * *

</div>

You can create you own themes and customize standart themes in very simple way.

#### Theme structure and files

magic's theme has 5 required files:

1.  **magic_container.php**
2.  **magic_list_view.php**
3.  **magic_detail_view.php**
4.  **magic.ini**
5.  **magic.css**

**magic_container.php** - this file is static container for ajax side of magic.  It can be non-styled, but all magic's event are delegated to this container. I not recommend to change something in this file.

**magic_list_view.php** - grid rendering.

**magic_detail_view.php** - rendering of create/edit/view screen

**magic.ini** - List of class names of core html elements, which cannot be changed in theme files.

**magic.css** - theme styles.

All this files will be loaded automatically.

#### Global variables in templates

**$mode** - shows current action. It can be **list**, **create**, **edit**, **view**.

#### Standart render methods

<div class="alert">Some of render methods can accept **tag** paremeter. It can be a simple string ('div','h1' etc) or array (array('tag'=>'div')). In array you can add any other html attributes (array('tag'=>'a','href'=>'http://example.com')).</div>

**csv_button(** _classname, icon_ **)** - render csv export button

**print_button(** _classname, icon_ **)** - render print button button

**add_button(** _classname, icon_ **)** - render Add button button

**render_limitlist(** _buttons_ **)** - render dropdown list or buttons with available rows per page. buttons = true - enable buttons instead of dropdown, default is false.

**render_pagination(** _numbers, offsets_ **)** - pagination; numbers - count of pagination buttons (default is 10); offsets -  first and last buttons offsets (default is 2)

**render_search()** - search form

**render_button(** _name, task, mode_after_task, classname, icon, primary_key_ **)** - sender system action button.

**render_benchmark(** _tag )_ - shows benchmark info. Recomended to put it in the end of template.

**render_grid_head(** _row_tag, item_tag, arrows_array_ **)** - return grid heading. Arrorws array contains arrows for ordered column. Default is **array('asc' => '&uarr; ', 'desc' => '&darr; ')**

**render_grid_body(** _row_tag, item_tag_ **)** - main grid rendering

**render_grid_footer(** _row_tag, item_tag_ **)** - renders grid footer if available ( eg sum row)

**render_fields_list(** _mode, container, row, label, field, tabs_container, tabs_menu_container, tabs_menu_row, tabs_menu_link, tabs_content_container, tabs_pane_ **)** - big method, creates details row grid and tabs. Default values: (**$mode, $container = 'table', $row = 'tr', $label = 'td', $field = 'td', $tabs_block = 'div', $tabs_head = 'ul', $tabs_row = 'li', $tabs_link = 'a', $tabs_content = 'div', $tabs_pane = 'div'**). $mode parameter is required.

**render_table_name(** _mode, tag, minimized_ **)** - render table heading with table tooltip and toggle arrow. minimized - used only in container file.

#### Sending custom variables in template

**set_var(** _var_name, value_ **)** - set user variable

**get_var(** _var_name_ **)** - get user variable

**unset_var(** _var_name_ **)** - unset user variable

<div class="alert alert-info">In theme files current instance can be accessed as **$this**, e.g. **$this->get_var('my_var');**</div>

#### Relocating template files

**load_view(** _task, file_ **)** - this method allows you to replace default view file by your custom by task for current instance.

1.  <span class="pln">$app-></span><span class="pln">load_view</span><span class="pun">(</span><span class="str">'create'</span><span class="pun">,</span><span class="str">'my_custom_create_form.php'</span><span class="pun">);</span>

Your custom theme files must be in your theme folder.

#### Creating non-standart rendering

**$this->result_list** - array of arrays with full result from database (grid). You can use it to create your own grid rendering.

**$this->result_row** - array with full result form database (create/edit/view).

**$this->fields_output** - array with rendered fields, exept hidden and additional fields. Every element contains:

1.  _label_ - field name, generated by magic or defined by label() method
2.  _name_ - full field name, used by magic.
3.  _value_ - raw value from database
4.  _field_ - already rendered form field by magic.

**$this->hidden_fields_output** - array of rendered hidden fields

</section>

<section id="2_date_and_time_formats">

<div class="page-header">

### 7\. Date and time formats

* * *

</div>

#### PHP date format

**Link: [http://www.php.net/manual/en/function.date.php](http://www.php.net/manual/en/function.date.php)**

The format of the outputted date <span class="type">[string](http://www.php.net/manual/en/language.types.string.php)</span>. See the formatting options below. There are also several [predefined date constants](http://www.php.net/manual/en/class.datetime.php#datetime.constants.types) that may be used instead, so for example **`DATE_RSS`** contains the format string _'D, d M Y H:i:s'_.

<table class="doctable table"><caption>**The following characters are recognized in the _`format`_ parameter string**</caption>

<thead>

<tr>

<th>_`format`_ character</th>

<th>Description</th>

<th>Example returned values</th>

</tr>

</thead>

<tbody class="tbody">

<tr>

<td style="text-align: center;">_Day_</td>

<td>---</td>

<td>---</td>

</tr>

<tr>

<td>_d_</td>

<td>Day of the month, 2 digits with leading zeros</td>

<td>_01_ to _31_</td>

</tr>

<tr>

<td>_D_</td>

<td>A textual representation of a day, three letters</td>

<td>_Mon_ through _Sun_</td>

</tr>

<tr>

<td>_j_</td>

<td>Day of the month without leading zeros</td>

<td>_1_ to _31_</td>

</tr>

<tr>

<td>_l_ (lowercase 'L')</td>

<td>A full textual representation of the day of the week</td>

<td>_Sunday_ through _Saturday_</td>

</tr>

<tr>

<td>_N_</td>

<td>ISO-8601 numeric representation of the day of the week (added in PHP 5.1.0)</td>

<td>_1_ (for Monday) through _7_ (for Sunday)</td>

</tr>

<tr>

<td>_S_</td>

<td>English ordinal suffix for the day of the month, 2 characters</td>

<td>_st_, _nd_, _rd_ or _th_. Works well with _j_</td>

</tr>

<tr>

<td>_w_</td>

<td>Numeric representation of the day of the week</td>

<td>_0_ (for Sunday) through _6_ (for Saturday)</td>

</tr>

<tr>

<td>_z_</td>

<td>The day of the year (starting from 0)</td>

<td>_0_ through _365_</td>

</tr>

<tr>

<td style="text-align: center;">_Week_</td>

<td>---</td>

<td>---</td>

</tr>

<tr>

<td>_W_</td>

<td>ISO-8601 week number of year, weeks starting on Monday (added in PHP 4.1.0)</td>

<td>Example: _42_ (the 42nd week in the year)</td>

</tr>

<tr>

<td style="text-align: center;">_Month_</td>

<td>---</td>

<td>---</td>

</tr>

<tr>

<td>_F_</td>

<td>A full textual representation of a month, such as January or March</td>

<td>_January_ through _December_</td>

</tr>

<tr>

<td>_m_</td>

<td>Numeric representation of a month, with leading zeros</td>

<td>_01_ through _12_</td>

</tr>

<tr>

<td>_M_</td>

<td>A short textual representation of a month, three letters</td>

<td>_Jan_ through _Dec_</td>

</tr>

<tr>

<td>_n_</td>

<td>Numeric representation of a month, without leading zeros</td>

<td>_1_ through _12_</td>

</tr>

<tr>

<td>_t_</td>

<td>Number of days in the given month</td>

<td>_28_ through _31_</td>

</tr>

<tr>

<td style="text-align: center;">_Year_</td>

<td>---</td>

<td>---</td>

</tr>

<tr>

<td>_L_</td>

<td>Whether it's a leap year</td>

<td>_1_ if it is a leap year, _0_ otherwise.</td>

</tr>

<tr>

<td>_o_</td>

<td>ISO-8601 year number. This has the same value as _Y_, except that if the ISO week number (_W_) belongs to the previous or next year, that year is used instead. (added in PHP 5.1.0)</td>

<td>Examples: _1999_ or _2003_</td>

</tr>

<tr>

<td>_Y_</td>

<td>A full numeric representation of a year, 4 digits</td>

<td>Examples: _1999_ or _2003_</td>

</tr>

<tr>

<td>_y_</td>

<td>A two digit representation of a year</td>

<td>Examples: _99_ or _03_</td>

</tr>

<tr>

<td style="text-align: center;">_Time_</td>

<td>---</td>

<td>---</td>

</tr>

<tr>

<td>_a_</td>

<td>Lowercase Ante meridiem and Post meridiem</td>

<td>_am_ or _pm_</td>

</tr>

<tr>

<td>_A_</td>

<td>Uppercase Ante meridiem and Post meridiem</td>

<td>_AM_ or _PM_</td>

</tr>

<tr>

<td>_B_</td>

<td>Swatch Internet time</td>

<td>_000_ through _999_</td>

</tr>

<tr>

<td>_g_</td>

<td>12-hour format of an hour without leading zeros</td>

<td>_1_ through _12_</td>

</tr>

<tr>

<td>_G_</td>

<td>24-hour format of an hour without leading zeros</td>

<td>_0_ through _23_</td>

</tr>

<tr>

<td>_h_</td>

<td>12-hour format of an hour with leading zeros</td>

<td>_01_ through _12_</td>

</tr>

<tr>

<td>_H_</td>

<td>24-hour format of an hour with leading zeros</td>

<td>_00_ through _23_</td>

</tr>

<tr>

<td>_i_</td>

<td>Minutes with leading zeros</td>

<td>_00_ to _59_</td>

</tr>

<tr>

<td>_s_</td>

<td>Seconds, with leading zeros</td>

<td>_00_ through _59_</td>

</tr>

<tr>

<td>_u_</td>

<td>Microseconds (added in PHP 5.2.2). Note that <span class="function">**date()**</span> will always generate _000000_ since it takes an <span class="type">[integer](http://www.php.net/manual/en/language.types.integer.php)</span> parameter, whereas <span class="methodname">[DateTime::format()](http://www.php.net/manual/en/datetime.format.php)</span> does support microseconds.</td>

<td>Example: _654321_</td>

</tr>

<tr>

<td style="text-align: center;">_Timezone_</td>

<td>---</td>

<td>---</td>

</tr>

<tr>

<td>_e_</td>

<td>Timezone identifier (added in PHP 5.1.0)</td>

<td>Examples: _UTC_, _GMT_, _Atlantic/Azores_</td>

</tr>

<tr>

<td>_I_ (capital i)</td>

<td>Whether or not the date is in daylight saving time</td>

<td>_1_ if Daylight Saving Time, _0_ otherwise.</td>

</tr>

<tr>

<td>_O_</td>

<td>Difference to Greenwich time (GMT) in hours</td>

<td>Example: _+0200_</td>

</tr>

<tr>

<td>_P_</td>

<td>Difference to Greenwich time (GMT) with colon between hours and minutes (added in PHP 5.1.3)</td>

<td>Example: _+02:00_</td>

</tr>

<tr>

<td>_T_</td>

<td>Timezone abbreviation</td>

<td>Examples: _EST_, _MDT_ ...</td>

</tr>

<tr>

<td>_Z_</td>

<td>Timezone offset in seconds. The offset for timezones west of UTC is always negative, and for those east of UTC is always positive.</td>

<td>_-43200_ through _50400_</td>

</tr>

<tr>

<td style="text-align: center;">_Full Date/Time_</td>

<td>---</td>

<td>---</td>

</tr>

<tr>

<td>_c_</td>

<td>ISO 8601 date (added in PHP 5)</td>

<td>2004-02-12T15:19:21+00:00</td>

</tr>

<tr>

<td>_r_</td>

<td>[» RFC 2822](http://www.faqs.org/rfcs/rfc2822) formatted date</td>

<td>Example: _Thu, 21 Dec 2000 16:01:07 +0200_</td>

</tr>

<tr>

<td>_U_</td>

<td>Seconds since the Unix Epoch (January 1 1970 00:00:00 GMT)</td>

<td>See also <span class="function">[time()](http://www.php.net/manual/en/function.time.php)</span></td>

</tr>

</tbody>

</table>

#### jQuery UI date format

Link: [http://api.jqueryui.com/datepicker/#utility-formatDate](http://api.jqueryui.com/datepicker/#utility-formatDate)

Link: [http://trentrichardson.com/examples/timepicker/](http://trentrichardson.com/examples/timepicker/)

Format a date into a string value with a specified format.

The format can be combinations of the following:

*   d - day of month (no leading zero)
*   dd - day of month (two digit)
*   o - day of the year (no leading zeros)
*   oo - day of the year (three digit)
*   D - day name short
*   DD - day name long
*   m - month of year (no leading zero)
*   mm - month of year (two digit)
*   M - month name short
*   MM - month name long
*   y - year (two digit)
*   yy - year (four digit)
*   @ - Unix timestamp (ms since 01/01/1970)
*   ! - Windows ticks (100ns since 01/01/0001)
*   '...' - literal text
*   '' - single quote
*   anything else - literal text

There are also a number of predefined standard date formats available from `$.datepicker`:

*   ATOM - 'yy-mm-dd' (Same as RFC 3339/ISO 8601)
*   COOKIE - 'D, dd M yy'
*   ISO_8601 - 'yy-mm-dd'
*   RFC_822 - 'D, d M y' (See RFC 822)
*   RFC_850 - 'DD, dd-M-y' (See RFC 850)
*   RFC_1036 - 'D, d M y' (See RFC 1036)
*   RFC_1123 - 'D, d M yy' (See RFC 1123)
*   RFC_2822 - 'D, d M yy' (See RFC 2822)
*   RSS - 'D, d M y' (Same as RFC 822)
*   TICKS - '!'
*   TIMESTAMP - '@'
*   W3C - 'yy-mm-dd' (Same as ISO 8601)

Time format

*   H - Hour with no leading 0 (24 hour)
*   HH - Hour with leading 0 (24 hour)
*   h - Hour with no leading 0 (12 hour)
*   hh - Hour with leading 0 (12 hour)
*   m - Minute with no leading 0
*   mm - Minute with leading 0
*   s - Second with no leading 0
*   ss - Second with leading 0
*   l - Milliseconds always with leading 0
*   c - Microseconds always with leading 0
*   t - a or p for AM/PM
*   T - A or P for AM/PM
*   tt - am or pm for AM/PM
*   TT - AM or PM for AM/PM
*   z - Timezone as defined by timezoneList
*   Z - Timezone in Iso 8601 format (+04:45)
*   '...' - Literal text (Uses single quotes)

</section>

</div>
