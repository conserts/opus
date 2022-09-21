<!--
SPDX-FileCopyrightText: 2022 Andreas Schmidt <andreas.schmidt@iese.fraunhofer.de>

SPDX-License-Identifier: CC-BY-SA-4.0
-->

# conserts-rs

[conserts-rs](https://github.com/conserts/conserts-rs) is a library and set of command-line tools for working with ConSert models.

The goal of this library is to allow both safety engineers as well as (embedded) software developers to work with ConSert models.
These models come in the form of files, generated e.g. via the [safeTbox software](https://safetbox.de/).
Usage of these models includes both runtime safety evaluation of single systems (where conserts-rs helps in auto-generating code) as well as the safety evaluation of the collaboration of several systems (where conserts-rs checks the composability).

## Functionality

`conserts-rs` supports the following workflows:

* Parsing (and validating) these models.
* Compiling Rust crates that contain logic for evaluating ConSerts at runtime. This crate is:
  * Usable for either
    * embedded targets that require `#![no_std]`,
    * [ROS](https://www.ros.org/) (Robot Operating System), as it brings ready to use ROS nodes along, or
    * any other system in which a binary generated from Rust can be deployed.
  * Unit-safe (with the help of [uom](https://crates.io/crates/uom)).
  * Usable through C/C++ bindings (with the help of [cbindgen](https://crates.io/crates/cbindgen)).
  * Automatically documented, by making connections from the ConSert model to the generated code.
* Validating the composition of several ConSert models.
* Visualizing the structure and current evaluation state of ConSert models.
