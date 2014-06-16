.. _control-types-and-modes:

Control types and modes
=======================

A Control is a term used to refer to a command sent from a master to an outstation. DNP3 provides for a number of control types including Control Relay Output Blocks (CROB) and integer / floating point encodings for Analog Outputs.

CROB is a flexible type for changing a binary state. It can be used to toggle, pulse, or create a digital waveform. Which type of CROB a device supports is device dependent and you'll have to consult your outstation's documentation to know exactly what to send. CROB corresponds to the DNP3 serialization Group 12 Variation 1.

AnalogOutputs (aka set-points) represent a discrete integer or continuous floating point output. This could represent the tap position of a transformer, a floating point voltage target, or an integer operating mode enumeration. What set-points do varies from device to device. The four types of AnalogOutputs are:

* AnalogOutputInt32 - 32-bit signed integer, Group 41 Variation 1
* AnalogOutputInt16 - 16-bit signed integer, Group 41 Variation 2
* AnalogOutputFloat32 - 32-bit single presicion floating point, Group 41 Variation 3
* AnalogOutputDouble64 - 64-bit double presicion floating point, Group 41 Variation 4

Independent of the control type you are requesting, is the mode of operation. DNP3 provdes 3 modes of operation:

* SelectBeforeOperate (SBO) - In this mode, the master sends two separate requests to the outstation with a SELECT function code followed by an OPERATE function code and identical data payloads. The outstation responds to each request. The master waits for the select response before operating. This is a safety precaution to avoid erroneous operations. It is somewhat antiquated over today's IP networks, but is still quite common in production.

* DirectOperate (DO) - In this mode, the master sends a single DIRECT_OPERATE request to the outstation and the control is performed immediately. The outstation responds to the request with a status enumeration.

* DirectOperateNoAck (DO no ack) - In this mode, the master sends a single DIRECT_OPERATE_NO_ACK request to the outstation and the control is performed immediately. The outstation does not acknowledge the request and silently performs it or fails it. 
