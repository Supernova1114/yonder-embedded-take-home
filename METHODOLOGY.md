# Methodology

## 1. How to run it

python3 sim/launch.py --visualize

## 2. Thought process

- For the gnss weight, I decided to make it based off of the covariance value.
- A linear weight based on a MAX_TRUSTABLE_COV value. If the covariance is small, then the gps weight has a higher trust, and if it is large the weight is smaller.
- Then I multiplied the weight output by MAX_W_GPS to reduce the weight of the gnss signal as the gnss signal seems decently noisy. It also limits the gnss weight to MAX_W_GPS.
- For the heading itself, I decided to average the directional vectors between consecutive gnss points in order to get an average direction given a HEADING_BUFFER_MAX_LEN number of points, and then convert the averaged vector into a heading of radians. 
- The benefit of the lower gps weight is that the odom does not get pulled around as much by the varying gnss points.
- The benefit of the heading averaging is that the heading is not as affected by noisy gnss positions. Where relying on just two points can for example cause the heading to flip 180 degrees if gnss noise were to put a point behind the last gnss sample.

## 3. Known limitations

One limitation of my implementation is that if the gnss signal cuts out for a few seconds, then comes back up, the odom position takes a larger number of gnss position samples in order to converge back to the estimated position, than would be required for a kalman filter for example. Part of the issue in my design is the lower weight given to the gnss, which causes the convergence to slow, and also the averaging for the heading, which changes the heading direction slower.
