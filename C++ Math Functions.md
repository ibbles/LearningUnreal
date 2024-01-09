# `FMath`

`FMath::Clamp` clamps a value to be within a given allowed range.
Values outside of that range are set to the closest bound on the allowed range.


`GetRangePct` is an inverse `lerp`. Given a range and a value within that range it returns a `[0.0 ... 1.0]` value describing where in the range the value is. If the value is outside the range then a value less than 0.0 or greater than 1.0 is returned.
