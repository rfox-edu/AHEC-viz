# Synthetic appointment dataset

**Entirely simulated teaching data.** These values were constructed to illustrate visual exploration and confounding. They are not benchmarks or findings about any real clinic, patient population, or reminder product.

The CSV contains **6,000 original scheduled appointments**, seven columns, four specialties, four age bands, and four reminder methods across one fictional calendar year. No patient names or identifiers are included.

Each row is the original booking of a distinct appointment slot. If an original appointment is canceled and the slot is filled by a replacement booking, the replacement is represented by `slot_refilled = Yes`; the replacement is **not another row**. This lets participants count slot capacity once.

| Column | Type | Allowed values / meaning |
|---|---|---|
| `month` | Ordered category | January, February, March, April, May, June, July, August, September, October, November, December. Month of the original appointment; sort in calendar order. |
| `specialty` | Nominal category | Primary Care, Cardiology, Behavioral Health, Orthopedics. |
| `age_band` | Ordered category | 18–34, 35–49, 50–64, 65+. Age at the original appointment; the CSV uses an en dash. |
| `booking_lead_days` | Integer, days | 0–90 inclusive. Elapsed days between original booking and the appointment. This measures the booked interval, not the independently observed first available appointment or patient demand. |
| `reminder_method` | Nominal category | Phone call, Text, AI outreach, Email. One reminder method per original appointment. |
| `appointment_outcome` | Nominal category | Completed, No-show, Canceled early, Canceled late. These outcomes are mutually exclusive and exhaustive. |
| `slot_refilled` | Nominal category | Yes, No, Not applicable. Yes/No applies only to canceled appointments. Completed and No-show always use Not applicable. |

**Fictional operational definitions:** A no-show is an appointment the original patient neither attended nor canceled before the scheduled visit. A completed appointment was attended by its original patient. Canceled early means canceled at least **24 hours** before the visit; canceled late means canceled less than 24 hours before the visit. Cancellation timing is encoded by the outcome label; exact hours are not in the CSV. These are exercise definitions, not clinical standards.

**Useful derived measures:**

| Measure | Numerator | Denominator |
|---|---|---|
| No-show rate | `appointment_outcome = No-show` | All original scheduled appointments in the group, including cancellations |
| Early/late cancellation rate | The corresponding canceled outcome | All original scheduled appointments in the group |
| Early/late refill rate | Canceled appointments of that type with `slot_refilled = Yes` | All canceled appointments of that type |
| Lost-slot rate | No-shows plus canceled appointments with `slot_refilled = No` | All original scheduled appointments in the group |
| Slot use rate | Completed original appointments plus canceled appointments with `slot_refilled = Yes` | All original scheduled appointments in the group |

Do not average group percentages to calculate the overall rate: sum matching numerators and denominators. The exception is an explicitly labeled standardization, which deliberately applies the same reference weights to groups for comparison.

For charts, derive four lead bands: **0–7, 8–21, 22–42, 43–90 days**. The bands are teaching choices, not clinical cutoffs. A pooled four-band/reminder cell contains at least 122 bookings. Some specialty × lead band × reminder cells are much smaller; retain counts when using those detailed slices.

The raw CSV has no blanks. Repeated combinations of values are expected because the file has no appointment identifier and only seven dimensions. Do not remove repeated rows as duplicates: they represent separate appointment slots.

The CSV is UTF-8 with a BOM for convenient opening in Microsoft Excel. Integer lead times are plain numeric values. Labels contain no commas.

The dataset can support descriptive comparisons, trends across booking lead time, and discussions of potential confounding. It cannot establish that age, reminder method, or changing appointment lead time caused an outcome. It also cannot measure revenue, appointment duration, clinical quality, patient satisfaction, or the next available appointment because those fields are absent.
