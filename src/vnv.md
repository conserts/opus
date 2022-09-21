<!--
SPDX-FileCopyrightText: 2022 Andreas Schmidt <andreas.schmidt@iese.fraunhofer.de>

SPDX-License-Identifier: CC-BY-SA-4.0
-->

# Verification and Validation

The preceding sections have dealt with the *left* side of the V-model and did not mention verfication and validation (V&V) activities in detail. This is the purpose of this section to show how we can V&V ConSerts.

When it comes to the engineering of ConSerts, we have several layers (top-to-bottom as in the V-model):

* ConSert Model Contents
* ConSert Model Transformation
* ConSert Runtime Monitor

## ConSert Model Contents

At the highest level, we deal with the ConSert model that formalizes the service-oriented safety relations between multiple systems.
In this step, we have to verify that the ConSert has the right elements, given a safety assessment of the collaborative systems.

## ConSert Model Transformation

At the middle level, the ConSert model is turned into Rust code elements, i.e. guarantees and their evaluation trees are rendered into `struct`s as well as `fn evaluate(g: Guarantee) -> bool`.
To validate this, a test kit for potential users can be used to prove equivalence of input models and output code.
Further, manual code review can be performed, as the code complexity can be handled well.
Our measurements yielded that in addition to static 150 lines of code (LoC) for the infrastructure, we have around 10 lines of code per element in the ConSert.
In total, this keeps the code to be reviewed to a tractable size.

## ConSert Runtime Monitor

Finally, we have to verify that the runtime monitor is correct and correctly transformed into an executable binary.
Again, code review allows to check for the correct generation of the monitor code.

Currently, the following steps (i.e. compilation) cannot yet be validated.
However, there is a long term effort to create [Ferrocene](https://ferrous-systems.com/ferrocene/), a verified Rust compiler.
Furthermore, it is intended to verify third-party Rust crates, such as heapless.
In summary, these activities would enable the verification of the binary, proving that a runtime monitor is generated that is faithful to the original ConSert model.
