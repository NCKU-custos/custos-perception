# custos-perception

Vision pipelines and sensor fusion for the Custos drone stack.

Depends only on [`custos-interfaces`](https://github.com/NCKU-custos/custos-interfaces) for the wire contract. Receives sensor data from the NOVATEK SDK wrapper (proprietary) or a mock node — code path is identical because both publish the same `custos_perception_msgs` topics.

For the system overview, see [`custos-bringup`](https://github.com/NCKU-custos/custos-bringup).
