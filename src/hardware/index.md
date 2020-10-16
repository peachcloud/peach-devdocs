# Hardware

PeachCloud is intended to be a low-cost device with a small footprint and relatively low power consumption. It should be easy to modify and extend through the inclusion of additional peripherals and hardware modules, either for future release by the PeachCloud team or by third-parties with basic technical competence. 

## Single-board Computer (SBC)

The Raspberry Pi 3B+ has been chosen as the development platform for PeachCloud. The 3B+ offers extensive documentation, a 64-bit processor, integrated wireless LAN, Bluetooth and BLE, as well as a 40-pin GPIO header. All of these features make it a powerful and flexible platform for system development.

It is important to note that a range of single-board computers (SBC) can be substituted in place of the 3B+, though some changes in system configuration may be required.

## Real-time Clock (RTC)

A real-time clock (RTC) module is included in the PeachCloud device as a means of keeping accurate system time, regardless of any power supply interruptions which may occur during deployment. The RTC module consists of an integrated circuit (IC) for time-keeping and a battery. Having a consistently-accurate system time will make it easier to schedule updates and prevent the end-user from having to update the time themselves.

## Physical Interface

PeachCloud includes a 128x64 pixel OLED display and several push-buttons. Together, these components form a physical interface which exposes a simple menu system for displaying system state and allowing basic device interactions. The physical interface is intended to be useful in contexts when browser-based interactions are not possible or desirable.
