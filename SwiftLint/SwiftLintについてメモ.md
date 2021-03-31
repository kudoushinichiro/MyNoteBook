
## 用語のメモ

- `included:` 記載のフォルダのソースのみ対象
- `excluded:` 対象から外すフォルダ
- `disabled_rules:` 無効にするルール
- `opt_in_rules:` デフォルト無効のルールのうち有効にするもの

rulesで`opt in`が`yes`になっているものがデフォルト無効ルール。yesが無効でややこしい。


## SwiftLint Rulues 一覧

```
swiftlint --version
0.43.1
```

```
 swiftlint rules
+------------------------------------------+--------+-------------+------------------------+-------------+----------+---------------+
| identifier                               | opt-in | correctable | enabled in your config | kind        | analyzer | configuration |
+------------------------------------------+--------+-------------+------------------------+-------------+----------+---------------+
| anyobject_protocol                       | yes    | yes         | no                     | lint        | no       | warning       |
| array_init                               | yes    | no          | no                     | lint        | no       | warning       |
| attributes                               | yes    | no          | no                     | style       | no       | warning, a... |
| balanced_xctest_lifecycle                | yes    | no          | no                     | lint        | no       | warning       |
| block_based_kvo                          | no     | no          | yes                    | idiomatic   | no       | warning       |
| capture_variable                         | yes    | no          | no                     | lint        | yes      | warning       |
| class_delegate_protocol                  | no     | no          | yes                    | lint        | no       | warning       |
| closing_brace                            | no     | yes         | yes                    | style       | no       | warning       |
| closure_body_length                      | yes    | no          | no                     | metrics     | no       | warning: 2... |
| closure_end_indentation                  | yes    | yes         | no                     | style       | no       | warning       |
| closure_parameter_position               | no     | no          | yes                    | style       | no       | warning       |
| closure_spacing                          | yes    | yes         | no                     | style       | no       | warning       |
| collection_alignment                     | yes    | no          | no                     | style       | no       | warning, a... |
| colon                                    | no     | yes         | yes                    | style       | no       | warning, f... |
| comma                                    | no     | yes         | yes                    | style       | no       | warning       |
| comment_spacing                          | no     | yes         | yes                    | lint        | no       | warning       |
| compiler_protocol_init                   | no     | no          | yes                    | lint        | no       | warning       |
| computed_accessors_order                 | no     | no          | yes                    | style       | no       | warning, o... |
| conditional_returns_on_newline           | yes    | no          | no                     | style       | no       | warning, i... |
| contains_over_filter_count               | yes    | no          | no                     | performance | no       | warning       |
| contains_over_filter_is_empty            | yes    | no          | no                     | performance | no       | warning       |
| contains_over_first_not_nil              | yes    | no          | no                     | performance | no       | warning       |
| contains_over_range_nil_comparison       | yes    | no          | no                     | performance | no       | warning       |
| control_statement                        | no     | yes         | yes                    | style       | no       | warning       |
| convenience_type                         | yes    | no          | no                     | idiomatic   | no       | warning       |
| custom_rules                             | no     | no          | no                     | style       | no       | user-defin... |
| cyclomatic_complexity                    | no     | no          | yes                    | metrics     | no       | warning: 1... |
| deployment_target                        | no     | no          | yes                    | lint        | no       | warning, i... |
| discarded_notification_center_observer   | yes    | no          | no                     | lint        | no       | warning       |
| discouraged_assert                       | yes    | no          | no                     | idiomatic   | no       | warning       |
| discouraged_direct_init                  | no     | no          | yes                    | lint        | no       | warning, t... |
| discouraged_object_literal               | yes    | no          | no                     | idiomatic   | no       | warning, i... |
| discouraged_optional_boolean             | yes    | no          | no                     | idiomatic   | no       | warning       |
| discouraged_optional_collection          | yes    | no          | no                     | idiomatic   | no       | warning       |
| duplicate_enum_cases                     | no     | no          | yes                    | lint        | no       | error         |
| duplicate_imports                        | no     | no          | yes                    | idiomatic   | no       | warning       |
| dynamic_inline                           | no     | no          | yes                    | lint        | no       | error         |
| empty_collection_literal                 | yes    | no          | no                     | performance | no       | warning       |
| empty_count                              | yes    | no          | no                     | performance | no       | error, onl... |
| empty_enum_arguments                     | no     | yes         | yes                    | style       | no       | warning       |
| empty_parameters                         | no     | yes         | yes                    | style       | no       | warning       |
| empty_parentheses_with_trailing_closure  | no     | yes         | yes                    | style       | no       | warning       |
| empty_string                             | yes    | no          | no                     | performance | no       | warning       |
| empty_xctest_method                      | yes    | no          | no                     | lint        | no       | warning       |
| enum_case_associated_values_count        | yes    | no          | no                     | metrics     | no       | warning: 5... |
| expiring_todo                            | yes    | no          | no                     | lint        | no       | (approachi... |
| explicit_acl                             | yes    | no          | no                     | idiomatic   | no       | warning       |
| explicit_enum_raw_value                  | yes    | no          | no                     | idiomatic   | no       | warning       |
| explicit_init                            | yes    | yes         | no                     | idiomatic   | no       | warning       |
| explicit_self                            | yes    | yes         | no                     | style       | yes      | warning       |
| explicit_top_level_acl                   | yes    | no          | no                     | idiomatic   | no       | warning       |
| explicit_type_interface                  | yes    | no          | no                     | idiomatic   | no       | warning, e... |
| extension_access_modifier                | yes    | no          | no                     | idiomatic   | no       | warning       |
| fallthrough                              | yes    | no          | no                     | idiomatic   | no       | warning       |
| fatal_error_message                      | yes    | no          | no                     | idiomatic   | no       | warning       |
| file_header                              | yes    | no          | no                     | style       | no       | warning, r... |
| file_length                              | no     | no          | yes                    | metrics     | no       | warning: 4... |
| file_name                                | yes    | no          | no                     | idiomatic   | no       | (severity)... |
| file_name_no_space                       | yes    | no          | no                     | idiomatic   | no       | (severity)... |
| file_types_order                         | yes    | no          | no                     | style       | no       | warning, o... |
| first_where                              | yes    | no          | no                     | performance | no       | warning       |
| flatmap_over_map_reduce                  | yes    | no          | no                     | performance | no       | warning       |
| for_where                                | no     | no          | yes                    | idiomatic   | no       | warning       |
| force_cast                               | no     | no          | yes                    | idiomatic   | no       | error         |
| force_try                                | no     | no          | yes                    | idiomatic   | no       | error         |
| force_unwrapping                         | yes    | no          | no                     | idiomatic   | no       | warning       |
| function_body_length                     | no     | no          | yes                    | metrics     | no       | warning: 4... |
| function_default_parameter_at_end        | yes    | no          | no                     | idiomatic   | no       | warning       |
| function_parameter_count                 | no     | no          | yes                    | metrics     | no       | warning: 5... |
| generic_type_name                        | no     | no          | yes                    | idiomatic   | no       | (min_lengt... |
| ibinspectable_in_extension               | yes    | no          | no                     | lint        | no       | warning       |
| identical_operands                       | yes    | no          | no                     | lint        | no       | warning       |
| identifier_name                          | no     | no          | yes                    | style       | no       | (min_lengt... |
| implicit_getter                          | no     | no          | yes                    | style       | no       | warning       |
| implicit_return                          | yes    | yes         | no                     | style       | no       | warning, i... |
| implicitly_unwrapped_optional            | yes    | no          | no                     | idiomatic   | no       | warning, m... |
| inclusive_language                       | no     | no          | yes                    | style       | no       | warning, a... |
| indentation_width                        | yes    | no          | no                     | style       | no       | severity: ... |
| inert_defer                              | no     | no          | yes                    | lint        | no       | warning       |
| is_disjoint                              | no     | no          | yes                    | idiomatic   | no       | warning       |
| joined_default_parameter                 | yes    | yes         | no                     | idiomatic   | no       | warning       |
| large_tuple                              | no     | no          | yes                    | metrics     | no       | warning: 2... |
| last_where                               | yes    | no          | no                     | performance | no       | warning       |
| leading_whitespace                       | no     | yes         | yes                    | style       | no       | warning       |
| legacy_cggeometry_functions              | no     | yes         | yes                    | idiomatic   | no       | warning       |
| legacy_constant                          | no     | yes         | yes                    | idiomatic   | no       | warning       |
| legacy_constructor                       | no     | yes         | yes                    | idiomatic   | no       | warning       |
| legacy_hashing                           | no     | no          | yes                    | idiomatic   | no       | warning       |
| legacy_multiple                          | yes    | no          | no                     | idiomatic   | no       | warning       |
| legacy_nsgeometry_functions              | no     | yes         | yes                    | idiomatic   | no       | warning       |
| legacy_objc_type                         | yes    | no          | no                     | idiomatic   | no       | warning       |
| legacy_random                            | yes    | no          | no                     | idiomatic   | no       | warning       |
| let_var_whitespace                       | yes    | no          | no                     | style       | no       | warning       |
| line_length                              | no     | no          | yes                    | metrics     | no       | warning: 1... |
| literal_expression_end_indentation       | yes    | yes         | no                     | style       | no       | warning       |
| lower_acl_than_parent                    | yes    | no          | no                     | lint        | no       | warning       |
| mark                                     | no     | yes         | yes                    | lint        | no       | warning       |
| missing_docs                             | yes    | no          | no                     | lint        | no       | warning: o... |
| modifier_order                           | yes    | yes         | no                     | style       | no       | warning, p... |
| multiline_arguments                      | yes    | no          | no                     | style       | no       | warning, f... |
| multiline_arguments_brackets             | yes    | no          | no                     | style       | no       | warning       |
| multiline_function_chains                | yes    | no          | no                     | style       | no       | warning       |
| multiline_literal_brackets               | yes    | no          | no                     | style       | no       | warning       |
| multiline_parameters                     | yes    | no          | no                     | style       | no       | warning, a... |
| multiline_parameters_brackets            | yes    | no          | no                     | style       | no       | warning       |
| multiple_closures_with_trailing_closure  | no     | no          | yes                    | style       | no       | warning       |
| nesting                                  | no     | no          | yes                    | metrics     | no       | (type_leve... |
| nimble_operator                          | yes    | yes         | no                     | idiomatic   | no       | warning       |
| no_extension_access_modifier             | yes    | no          | no                     | idiomatic   | no       | error         |
| no_fallthrough_only                      | no     | no          | yes                    | idiomatic   | no       | warning       |
| no_grouping_extension                    | yes    | no          | no                     | idiomatic   | no       | warning       |
| no_space_in_method_call                  | no     | yes         | yes                    | style       | no       | warning       |
| notification_center_detachment           | no     | no          | yes                    | lint        | no       | warning       |
| nslocalizedstring_key                    | yes    | no          | no                     | lint        | no       | warning       |
| nslocalizedstring_require_bundle         | yes    | no          | no                     | lint        | no       | warning       |
| nsobject_prefer_isequal                  | no     | no          | yes                    | lint        | no       | warning       |
| number_separator                         | yes    | yes         | no                     | style       | no       | warning, m... |
| object_literal                           | yes    | no          | no                     | idiomatic   | no       | warning, i... |
| opening_brace                            | no     | yes         | yes                    | style       | no       | warning, a... |
| operator_usage_whitespace                | yes    | yes         | no                     | style       | no       | warning, l... |
| operator_whitespace                      | no     | no          | yes                    | style       | no       | warning       |
| optional_enum_case_matching              | yes    | yes         | no                     | style       | no       | warning       |
| orphaned_doc_comment                     | no     | no          | yes                    | lint        | no       | warning       |
| overridden_super_call                    | yes    | no          | no                     | lint        | no       | warning, e... |
| override_in_extension                    | yes    | no          | no                     | lint        | no       | warning       |
| pattern_matching_keywords                | yes    | no          | no                     | idiomatic   | no       | warning       |
| prefer_nimble                            | yes    | no          | no                     | idiomatic   | no       | warning       |
| prefer_self_type_over_type_of_self       | yes    | yes         | no                     | style       | no       | warning       |
| prefer_zero_over_explicit_init           | yes    | yes         | no                     | idiomatic   | no       | warning       |
| prefixed_toplevel_constant               | yes    | no          | no                     | style       | no       | warning, o... |
| private_action                           | yes    | no          | no                     | lint        | no       | warning       |
| private_outlet                           | yes    | no          | no                     | lint        | no       | warning, a... |
| private_over_fileprivate                 | no     | yes         | yes                    | idiomatic   | no       | warning, v... |
| private_subject                          | yes    | no          | no                     | lint        | no       | warning       |
| private_unit_test                        | no     | no          | yes                    | lint        | no       | warning: X... |
| prohibited_interface_builder             | yes    | no          | no                     | lint        | no       | warning       |
| prohibited_super_call                    | yes    | no          | no                     | lint        | no       | warning, e... |
| protocol_property_accessors_order        | no     | yes         | yes                    | style       | no       | warning       |
| quick_discouraged_call                   | yes    | no          | no                     | lint        | no       | warning       |
| quick_discouraged_focused_test           | yes    | no          | no                     | lint        | no       | warning       |
| quick_discouraged_pending_test           | yes    | no          | no                     | lint        | no       | warning       |
| raw_value_for_camel_cased_codable_enum   | yes    | no          | no                     | lint        | no       | warning       |
| reduce_boolean                           | no     | no          | yes                    | performance | no       | warning       |
| reduce_into                              | yes    | no          | no                     | performance | no       | warning       |
| redundant_discardable_let                | no     | yes         | yes                    | style       | no       | warning       |
| redundant_nil_coalescing                 | yes    | yes         | no                     | idiomatic   | no       | warning       |
| redundant_objc_attribute                 | no     | yes         | yes                    | idiomatic   | no       | warning       |
| redundant_optional_initialization        | no     | yes         | yes                    | idiomatic   | no       | warning       |
| redundant_set_access_control             | no     | no          | yes                    | idiomatic   | no       | warning       |
| redundant_string_enum_value              | no     | no          | yes                    | idiomatic   | no       | warning       |
| redundant_type_annotation                | yes    | yes         | no                     | idiomatic   | no       | warning       |
| redundant_void_return                    | no     | yes         | yes                    | idiomatic   | no       | warning       |
| required_deinit                          | yes    | no          | no                     | lint        | no       | warning       |
| required_enum_case                       | yes    | no          | no                     | lint        | no       | No protoco... |
| return_arrow_whitespace                  | no     | yes         | yes                    | style       | no       | warning       |
| shorthand_operator                       | no     | no          | yes                    | style       | no       | error         |
| single_test_class                        | yes    | no          | no                     | style       | no       | warning       |
| sorted_first_last                        | yes    | no          | no                     | performance | no       | warning       |
| sorted_imports                           | yes    | yes         | no                     | style       | no       | warning       |
| statement_position                       | no     | yes         | yes                    | style       | no       | (statement... |
| static_operator                          | yes    | no          | no                     | idiomatic   | no       | warning       |
| strict_fileprivate                       | yes    | no          | no                     | idiomatic   | no       | warning       |
| strong_iboutlet                          | yes    | yes         | no                     | lint        | no       | warning       |
| superfluous_disable_command              | no     | no          | yes                    | lint        | no       | warning       |
| switch_case_alignment                    | no     | no          | yes                    | style       | no       | warning, i... |
| switch_case_on_newline                   | yes    | no          | no                     | style       | no       | warning       |
| syntactic_sugar                          | no     | yes         | yes                    | idiomatic   | no       | warning       |
| test_case_accessibility                  | yes    | yes         | no                     | lint        | no       | warning, a... |
| todo                                     | no     | no          | yes                    | lint        | no       | warning       |
| toggle_bool                              | yes    | yes         | no                     | idiomatic   | no       | warning       |
| trailing_closure                         | yes    | no          | no                     | style       | no       | warning, o... |
| trailing_comma                           | no     | yes         | yes                    | style       | no       | warning, m... |
| trailing_newline                         | no     | yes         | yes                    | style       | no       | warning       |
| trailing_semicolon                       | no     | yes         | yes                    | idiomatic   | no       | warning       |
| trailing_whitespace                      | no     | yes         | yes                    | style       | no       | warning, i... |
| type_body_length                         | no     | no          | yes                    | metrics     | no       | warning: 2... |
| type_contents_order                      | yes    | no          | no                     | style       | no       | warning, o... |
| type_name                                | no     | no          | yes                    | idiomatic   | no       | (min_lengt... |
| unavailable_function                     | yes    | no          | no                     | idiomatic   | no       | warning       |
| unneeded_break_in_switch                 | no     | no          | yes                    | idiomatic   | no       | warning       |
| unneeded_parentheses_in_closure_argument | yes    | yes         | no                     | style       | no       | warning       |
| unowned_variable_capture                 | yes    | no          | no                     | lint        | no       | warning       |
| untyped_error_in_catch                   | yes    | yes         | no                     | idiomatic   | no       | warning       |
| unused_capture_list                      | no     | no          | yes                    | lint        | no       | warning       |
| unused_closure_parameter                 | no     | yes         | yes                    | lint        | no       | warning       |
| unused_control_flow_label                | no     | yes         | yes                    | lint        | no       | warning       |
| unused_declaration                       | yes    | no          | no                     | lint        | yes      | severity: ... |
| unused_enumerated                        | no     | no          | yes                    | idiomatic   | no       | warning       |
| unused_import                            | yes    | yes         | no                     | lint        | yes      | severity: ... |
| unused_optional_binding                  | no     | no          | yes                    | style       | no       | warning, i... |
| unused_setter_value                      | no     | no          | yes                    | lint        | no       | warning       |
| valid_ibinspectable                      | no     | no          | yes                    | lint        | no       | warning       |
| vertical_parameter_alignment             | no     | no          | yes                    | style       | no       | warning       |
| vertical_parameter_alignment_on_call     | yes    | no          | no                     | style       | no       | warning       |
| vertical_whitespace                      | no     | yes         | yes                    | style       | no       | warning, m... |
| vertical_whitespace_between_cases        | yes    | yes         | no                     | style       | no       | warning       |
| vertical_whitespace_closing_braces       | yes    | yes         | no                     | style       | no       | N/A           |
| vertical_whitespace_opening_braces       | yes    | yes         | no                     | style       | no       | N/A           |
| void_return                              | no     | yes         | yes                    | style       | no       | warning       |
| weak_delegate                            | no     | yes         | yes                    | lint        | no       | warning       |
| xct_specific_matcher                     | yes    | no          | no                     | idiomatic   | no       | warning       |
| xctfail_message                          | no     | no          | yes                    | idiomatic   | no       | warning       |
| yoda_condition                           | yes    | no          | no                     | lint        | no       | warning       |
+------------------------------------------+--------+-------------+------------------------+-------------+----------+---------------+


```
