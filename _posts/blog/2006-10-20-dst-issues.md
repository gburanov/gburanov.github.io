---
layout: post
title: "DST issues"
modified:
categories: blog
excerpt:
tags: [cpp, scheduling]
image:
  feature:
date: 2006-10-20T00:00:00
---
# DST issues

The first bug I got in new team - time issues when switching from DST to non-DST time. I found this bug simply by showing the calendar for half a year. The calendar looked really fine, but there were two October, 25 in it. I looked at the code that increments the day

```
byte SchedulerTime::IncDay(int dayCount /* = 1 */)
{
  LinearTicks() += TicksPerDay * dayCount;
  FillComponent();
  return GetDay();
}
```

The code looked fine at the first glance. The internal data structure for CommonTime is number of milliseconds since January (on Windows, different value on Mac, Linux). What’s wrong with 25 October? Correct answer – in Russia this day is switching day from DST to non-DST time. And yes, the 25 October is the special day that lasts 25, not 24 hours.

How should we fix that bug? We should check the UTC offset. The UTC offsets shows us the difference between UTC time and our local time.For example, for Moscow the offset is +3/+4 depending on DST/non-DST time. The corrected code looks the following:

```
byte SchedulerTime::IncDay(int dayCount /* = 1 */)
{
  uni_time_t previousOffset = GetUtcOffset();
  LinearTicks() += TicksPerDay * dayCount;
  FillComponent();
  FixTimeWhenShiftingUtcOffset(previousOffset);
  return GetDay();
}
```

The implementation of FixTimeWhenShiftingUtcOffset is:

```
/**
* @description fixes time when there was a dst/nondst switch during incrementing/decrementing day
* @param previousOffset UTC time offset before the incremention/decremention
*/
void SchedulerTime::FixTimeWhenShiftingUtcOffset(const uni_time_t& previousOffset)
{
 if (!UseUtc)
 {
   uni_time_t currentOffset = GetUtcOffset();
   if (currentOffset != previousOffset)
   {
     LinearTime += currentOffset - previousOffset;
     FillComponent();
   }
 }
}
```

The next question is: If we schedule task to be executed every day once at 2.30, should the task execute twice during DST to non-DST switch (because we have 2.30 twice) or should it execute once (because the man wanted to execute it every day). What is your answer? Why?
