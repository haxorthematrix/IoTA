[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Industrial IoT (IIoT) and OT Convergence**

The convergence of IT and OT networks means that IoT devices
increasingly bridge into industrial control system environments. The
IoTA team should evaluate industrial protocol security, network
segmentation, and the potential for IoT devices to serve as entry
points into critical infrastructure.

### Industrial Protocol Testing

The security of industrial protocols should be evaluated, including
Modbus TCP/RTU, OPC UA, EtherNet/IP (CIP), PROFINET, BACnet (building
automation), and DNP3. For IoT devices that interact with or act as
programmable logic controllers (PLCs) or remote terminal units (RTUs),
the team should evaluate default credentials, firmware update security,
logic manipulation, and unauthorized read/write of process values.

### Safety and Segmentation

Any finding that could impact Safety Instrumented System (SIS)
integrity should be treated as critical regardless of other risk
factors. Adherence to the Purdue Model (ISA-95) for network
segmentation should be evaluated, testing whether IoT devices properly
reside within their designated network zones and whether they can be
used to traverse zone boundaries. Zero trust microsegmentation
principles should be assessed, evaluating whether compromised IoT
devices can access resources beyond their minimum required scope.
