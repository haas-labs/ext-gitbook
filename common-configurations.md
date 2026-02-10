---
description: Common Detectors Configurations
---

# Common Configurations

### Severity

Alert (Event) Severity levels

<table><thead><tr><th width="234.859375">Level</th><th width="395.4246826171875">Description</th><th>Comment</th></tr></thead><tbody><tr><td><mark style="color:$danger;">CRITICAL</mark></td><td>Critcial Event. Needs immediate reaction or automatica action (incident)</td><td></td></tr><tr><td><mark style="color:orange;">HIGH</mark></td><td>Dangerous Event. Needs attention and resolution</td><td></td></tr><tr><td><mark style="color:yellow;">MEDIUM</mark></td><td>Important or Suspicious Event. </td><td></td></tr><tr><td><mark style="color:blue;">LOW</mark></td><td>Useful information Event with may provide insights or metrics</td><td></td></tr><tr><td><mark style="color:$info;">INFO</mark></td><td>Information only Event, often used for notification of the progress or state</td><td></td></tr><tr><td>Auto</td><td>Severity is determined by the Detector itself, based on Detector's functionality and intelligence.<br>In <code>Auto</code> severity mode, Detector may generate Alerts (Events) with different severities. If such intelligent Detector severity is overrided, all Alerts will be with overriden severity level</td><td></td></tr></tbody></table>

***

### Cron

Cron Expression Configuration

<table><thead><tr><th width="245.539794921875">Expression</th><th width="289.7244873046875">Description</th><th>Examples</th></tr></thead><tbody><tr><td>Quartz</td><td><a href="https://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html">Quarz Cron</a> expresson Syntax</td><td><pre><code>0 0 12 * * ?
</code></pre></td></tr><tr><td>Human</td><td>Simple Expression in human readable format interval</td><td><code>10 min</code>, <code>1 day</code>, 2 <code>hours</code>, </td></tr><tr><td></td><td></td><td></td></tr><tr><td>Milliseconds</td><td>Millisecond value interval</td><td><code>60000</code></td></tr></tbody></table>

***

### Condition (Threshold)

Condition (Threshold) Configuration

```scala
1. BASIC COMPARISON OPERATORS:
   = value      -> equals value (exact match)
   != value     -> not equals value (exact match)

2. ABSOLUTE VALUE HANDLING:
   > 10         -> v > 10 OR v < -10 (absolute value handling)
   < 25         -> |v| < 25 (absolute value handling)
   >= 35        -> |v| >= 35 (absolute value handling)
   <= 50        -> |v| <= 50 (absolute value handling)

3. EXPLICIT SIGN HANDLING (no absolute value):
   > +10        -> v > 10 (exact value, no absolute handling)
   > -15        -> v > -15 (exact value, no absolute handling)
   < +30        -> v < 30 (exact value, no absolute handling)
   < -20        -> v < -20 (exact value, no absolute handling)

3.5. ONCE OPERATORS (edge detection - trigger only when condition transitions from false to true):
   >> 10        -> v > 10 (triggers only once when condition becomes true, then false on subsequent evaluations)
   << 25        -> v < 25 (triggers only once when condition becomes true, then false on subsequent evaluations)
   >>= 10       -> v >= 10 (triggers only once when condition becomes true, then false on subsequent evaluations)
   <<= 25       -> v <= 25 (triggers only once when condition becomes true, then false on subsequent evaluations)
   >> ++100     -> increase > 100 (triggers only once when the increase condition becomes true)
   << --50      -> decrease > 50 (triggers only once when the decrease condition becomes true)
   >> 25%       -> percentage change > 25% (triggers only once when the percentage condition becomes true)
   << 10%       -> percentage change < 10% (triggers only once when the percentage condition becomes true)
   
   Note: These operators return true only when the condition transitions from false to true (edge detection).
         Once the condition becomes true, subsequent evaluations return false even if the condition remains true.
         This is useful for detecting threshold crossings or one-time events.

3.6. MAX/MIN OPERATORS (trigger only on new maximum/minimum):
   >>> 100      -> v > 100 AND v > previous max (triggers only when a new maximum is reached)
   <<< 50       -> v < 50 AND v < previous min (triggers only when a new minimum is reached)
   >>>= 100     -> v >= 100 AND v >= previous max (triggers only when a new maximum is reached, including equality)
   <<<= 50      -> v <= 50 AND v <= previous min (triggers only when a new minimum is reached, including equality)
   
   Note: These operators track the maximum (>>>/>>>=) or minimum (<<</<<<=) value seen so far.
         They return true only when:
         - The value exceeds/meets (>>>/>>>=) or is below/meets (<<</<<<=) the threshold, AND
         - The value is greater than/equal (>>>/>>>=) or less than/equal (<<</<<<=) all previously seen values that met the threshold.
         Example for ">>> 100":
         - v=90 -> false (90 < 100)
         - v=200 -> true (200 > 100, and it's the first value exceeding threshold)
         - v=150 -> false (150 > 100, but 150 < 200, so not a new max)
         - v=300 -> true (300 > 100 AND 300 > 200, so new max)
         - v=200 -> false (200 > 100, but 200 < 300, so not a new max)
         Example for ">>>= 100":
         - v=100 -> true (100 >= 100, and it's the first value meeting threshold)
         - v=200 -> true (200 >= 100 AND 200 >= 100, so new max)
         - v=150 -> false (150 >= 100, but 150 < 200, so not a new max)
         - v=200 -> false (200 >= 100, but 200 <= 200, so not a new max)

4. PERCENTAGE OPERATIONS:
   > 25%        -> percentage change > 25% (absolute value handling)
   < 10%        -> percentage change < 10% (absolute value handling)
   >= 50%       -> percentage change >= 50% (absolute value handling)
   <= 75%       -> percentage change <= 75% (absolute value handling)
   = 100%       -> percentage change = 100% (exact match)
   != 200%      -> percentage change != 200% (exact match)
   
   Note: This calculates the percentage change from previous value to current value.
   Formula: (v - v0) / v0100. For example:
   - v0=1000, v=700: percentage change = (700-1000)/1000*100 = -30% (decreased by 30%)
   - v0=1000, v=1300: percentage change = (1300-1000)/1000*100 = 30% (increased by 30%)
   - v0=100, v=100: percentage change = (100-100)/100*100 = 0% (no change)

5. DELTA OPERATIONS (change from previous value):
   > ++100      -> increase > 100 (absolute change)
   < --50       -> decrease > 50 (magnitude of decrease)
   >= ++30      -> increase >= 30 (absolute change)
   <= --25      -> decrease >= 25 (magnitude of decrease)
   = ++20       -> increase = 20 (exact change)
   != ++40      -> increase != 40 (exact change)

6. PERCENTAGE DELTA OPERATIONS:
   > ++25%      -> increase of percentage change > 25%
   < --10%      -> increase of percentage change > 10%
   >= ++50%     -> increase of percentage change >= 50%
   <= --30%     -> increase of percentage change >= 30%
   = ++100%     -> increase of percentage change = 100%
   != ++200%    -> increase of percentage change != 200%

7. SPACES SUPPORT:
   > ++ 100     -> increase > 100 (spaces are ignored)
   <= -- 25     -> decrease >= 25 (spaces are ignored)
   = ++ 30.0 %  -> increase = 30% (spaces are ignored)


NOTES:
- Equality (=) and not equality (!=) always use exact value matching
- Comparison operators (>, <, >=, <=) respect absolute value handling
- Once operators (>>, <<) trigger only when condition transitions from false to true (edge detection)
- Delta operations (++, --) work with all comparison operators including once operators
- Percentage operations work with all comparison operators including once operators
- Spaces are automatically removed during parsing
- The abs parameter is automatically detected based on value prefix: + / - will not use absolute value handling
```
