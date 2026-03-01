# 5V Sensor Supply Upgrade: Cam Sensor and Oil Pressure Sensor Issues

*Source: rusEFI Discord, 2026-02-28 | Channel: 1356732771325968630*
*Contributors: @Bearded_Bogan*

## Summary

After upgrading sensor supply voltage to 5V, a user reported that neither cam sensor appeared to be working and the oil pressure sensor was giving erroneous readings. The crank trigger signal was also in question.

## Details

@Bearded_Bogan posted asking for verification of crank trigger signal quality following a voltage upgrade to 5V on the sensor supply rail, noting two concurrent issues:

- **Oil pressure sensor malfunction** — giving incorrect/erratic readings after the 5V upgrade
- **Both cam sensors not working** — neither cam position sensor appearing to produce a valid signal

The message was directed at another community member (pinging them by ID) and no diagnostic follow-up was captured in this session's log. The crank trigger signal log was apparently shared visually (attachment) for review.

## Notes

- Upgrading sensor supply voltage to 5V can affect sensors that were previously running on a different reference voltage — some oil pressure senders are not designed for a 5V reference (resistive senders need a specific pull-up supply; some are ratiometric 5V, others are 0-5V absolute). Verify the oil pressure sensor type before changing its supply voltage.
- Dual cam sensor failure after a supply voltage change could indicate the sensors require a specific voltage range, or that the common supply/ground is wired incorrectly.
- This thread was unresolved in the captured log — consider following up in channel 1356732771325968630 for outcome.
