<!--
SPDX-FileCopyrightText: 2022 Andreas Schmidt <andreas.schmidt@iese.fraunhofer.de>

SPDX-License-Identifier: CC-BY-SA-4.0
-->

# Composition Time

When ConSerts have been created and stored together with the systems they certify, we enter the runtime.
At runtime, we have emergent collaborations between multiple systems that each carry their ConSert.
In this scenario, the composition is used to assess the safety of the collaboration.

To achieve this, the collaborating ConSerts are put into a directed acyclic systems graph.
For this graph, the following rules apply:

* First, ConSerts without demands become leaf-nodes of the systems' tree.
* ConSerts with demands become additional nodes of the tree. For each them, the following applies:
  * We consider all demands of the ConSert and search for other nodes in the systems' tree with matching guarantees.
  * For each matching node, we insert a directed edge from the node satisfying the demand to the node with the demand.
  * If the node we are about to insert has at least one demand that cannot be fulfilled at all, we fail to insert it.
* Integrating ConSerts with demands can fail if done in the wrong order (i.e. the ConSert with the matching guarantee might not have been inserted yet). Due to the complexity of this approach, the order is left to the user and not figured out automatically.

Composition checks are done as follows:

* Guarantee and Demand must have the identical *functional service type*.
* The [dimensions](../conserts/dimensions.md) of both must be compatible:
  * For the dimension of the demand, there must be the matching dimension in the guarantee.
  * Only if both dimensions are binary, categorical, or numerical, they could be matching.
  * If they are categorical, the guarantee's possible values should at least be those required by the demand to be a match (or the other way round, depending on the [dimensions subset relationship](../conserts/dimensions.md)).
  * If they are numerical, the guarantee's range should at least include the range of the demand to be a match (or the other way round, see above).
