# Tunnelling SRT over QUIC Datagrams (Part 2)

TODO: Add introductory text here and link to Part 1 of the article.

### 4.3. Experimental Results

**Observed network characteristics**

Let us first look at the jitter (Fig. 4.3.1) and the number of packets received, lost, and reordered during the 3rd experiment  (see Fig. 4.3.2: Streaming via QUIC datagrams). While there is no latency management or recovery mechanisms implemented in QUIC (TODO: be more precise here), some of these metrics give us an idea about the network jitter and packet loss level.
As can be seen from Fig. 4.3.1, on average the Time-Stamped Delay Factor (TS-DF) equals 33.09 ms. However, there are regular spikes up to a maximum of 292.19 ms (red line). Such a high and variable jitter results in uneven, spiky packet delivery (Fig. 4.3.2, green line) and packet loss (Fig. 4.3.2, 4.3.4, red line), and leads to packet reordering (Fig. 4.3.2, 4.3.4, blue line).

![quic_3_jitter](img/quic_3_jitter.png)

Fig. 4.3.1: TS-DF vs RFC 3550 jitter at the server side when streaming via QUIC datagrams (Experiment 3).

![quic_3_packets](img/quic_3_packets.png)

Fig. 4.3.2: Number of received, lost, and reordered packets at the server side when streaming via QUIC datagrams (Experiment 3).

![quic_3_jitter_zoomed](img/quic_3_jitter_zoomed.png)

Fig. 4.3.3: Zoomed view of TS-DF vs RFC 3550 jitter at the server side when streaming via QUIC datagrams (Experiment 3).

![quic_3_packets_zoomed](img/quic_3_packets_zoomed.png)

Fig. 4.3.4: Zoomed view of received, lost, and reordered packets at the server side when streaming via QUIC datagrams (Experiment 3).

According to the data obtained for the first three experiments, the average packet loss ratio observed in the network is 0.001% (see Fig. 4.3.5, blue bars). This value corresponds to the one obtained in the 4th experiment where data has been collected for 1 hour (the data collection period for experiments 1 to 3 was 15 minutes).

![unrecovered_packets_vs_experiment](img/unrecovered_packets_vs_experiment.png)

Fig. 4.3.5: Percentage of unrecovered packets (or lost packets in the case of QUIC protocol) in each experiment.

The round-trip time observed during the 3rd experiment is on average 51.76 ms, with periodic spikes up to 237.44 ms (Fig. 4.3.6). These values correspond to experiment 4 where, over the course of 1 hour, average RTT equals 52.67 ms and maximum RTT reaches 245.49 ms respectively. It's worth noting that the following graph is built from the SRT protocol [`msRTT`](https://github.com/Haivision/srt/blob/master/docs/API/statistics.md#msrtt) statistic observed at the receiver side. 

TODO1: The graph was obtained with the help of srt-stats-plotting script
TODO2: In srt-stats-plotting there is a branch with script plotting QUIC statistics with quicly, for students maybe interesting).

![squic_3_rtt](img/squic_3_rtt.png)

Fig. 4.3.6: Smoothed round-trip time (in milliseconds) observed at the receiver side when streaming via SRT over QUIC (Experiment 3).

As can be seen from figures 4.3.1 and 4.3.6, periodic spikes in the round-trip time signal correspond to the spikes in the observed Time-Stamped Delay Factor and Jitter (RFC 3550) statistics.

**SRT helps to compensate for jitter and recover from packet loss and reordering**

Figure 4.3.7 shows the measurement results of the variations in end-to-end delay (or jitter) exhibited by the receiver during the 3rd experiment in the case of tunnelling SRT over QUIC. When compared with streaming over QUIC datagrams (Fig. 4.3.1), we can see that the network jitter inherent in QUIC traffic is efficiently smoothed by the SRT protocol. On average TS-DF equals 0.28 ms while the maximum observed value is 7.41 ms, which is significantly less than the values obtained for QUIC datagrams (33.09 ms and 292.19 ms, respectively).

![squic_3_jitter](img/squic_3_jitter.png)

Fig. 4.3.7: TS-DF vs RFC 3550 jitter at the server side when tunnelling via SRT over QUIC (Experiment 3).

Together with the latency management and jitter compensation mechanisms, SRT provides a number of methods to recover from packet loss and reordering. All this helps to achieve smooth and regular packet delivery to an upstream application (e.g., decoder) when using the SRT library (see Fig. 4.3.8, green line).

![squic_3_packets](img/squic_3_packets.png)

Fig. 4.3.8: Number of received, lost, and reordered packets at the server side when tunnelling via SRT over QUIC (Experiment 3).

It is important to note that the **_SRT latency_**, in milliseconds, should be properly configured before the transmission in order for the SRT protocol to have enough time for packet retransmission and loss recovery. The recommended value is usually 3-4 times the network round-trip time. However, under certain conditions, such as high network jitter due to spiky RTT signal behavior (see Fig. 4.3.1, 4.3.6), the recommended value might not be enough.

As can be seen in Fig. 4.3.9, the latency settings of 400 ms and 600 ms used in the test setup (as described in Section xx (TODO)) were not sufficient to fully recover from packet loss even though the unrecovered packet ratio is relatively small in both cases (0.000161% and 0.000033%, respectively). 

A latency setting of 800 ms, which takes into account the random RTT spikes up to 250 ms and more, seems to be a good starting point. The numbers obtained over a longer duration confirm this (see Fig. 4.3.5, Experiment 4).

![unrecovered_packets_vs_latency](img/unrecovered_packets_vs_latency.png)

Fig. 4.3.9: Percentage of unrecovered (or lost in the case of QUIC protocol) packets versus latency (Experiments 1-3 from left to right). 

Note that in Fig. 4.3.9 the latency values only only apply to the SRT over QUIC use case.

The following graph shows that SRT also helps to recover from the relatively small amount of packet reordering that occurs when streaming via QUIC datagrams. See Table 1 (TODO) for the exact numbers.

![reordered_packets_vs_experiment](img/reordered_packets_vs_experiment.png)

Fig. 4.3.10: Percentage of reordered packets in each experiment.

**Jitter comparison**

As can be seen from figures 4.3.11 and 4.3.12, for experiments 1-3 (15 minute duration) the SRT protocol helps to compensate for jitter inherent in the QUIC datagrams traffic and reduces the number of extreme values (marked with crosses) as well as the maximum TS-DF value observed during transmission. This allows the buffer size (required for smoothing out the network jitter) to be reduced at the receiver application side (e.g., decoder) .

The boxplot chart 4.3.12 shows that there is almost no difference between the medians of the first three groups when streaming over QUIC datagrams (see experiments 1-3, blue boxes). Since the notches in the boxplot do overlap, we can conclude with 95% confidence that the true medians for experiments 1-3 do not differ. The average jitter (marked with green triangles) varies within [32.79, 33.45] ms interval, and the spikes are of the same order, which can be also seen from Fig. 4.3.11 (grey crosses). See Table 3 for the detailed statistics.

When streaming via QUIC datagrams there is no difference between the first three experiments. The only parameter that changes is the SRT latency, which is applied only in the case of tunnelling SRT over QUIC. (TODO: SRT latency is doing what). When considering QUIC alone, experiments 1-3 can be considered to be equivalent.

![ts_df_vs_experiment](img/ts_df_vs_experiment.png)

Fig. 4.3.11a: Time-Stamped Delay Factor (TS-DF) in each experiment.

![ts_df_vs_latency](img/ts_df_vs_latency.png)

Fig. 4.3.11b: Time-Stamped Delay Factor (TS-DF) vs latency in experiments 1 to 3 (left to right).


TODO: here carefully, review once again) 

To ensure the statistical significance of our results we collected data for an hour in experiment 4 to compare with the data collected over 15-minute intervals in experiments 1-3. For experiment 4, the average TS-DF value equals 31.84 ms while the maximum observed value is 307.32 ms. The median equals 21.08 ms which is lower than for experiments 1-3 (1.59 ms, 1.45 ms, and 1.39 ms respectively). The notches for all experiments do not overlap, which indicates that for a longer time interval the jitter is significantly lower. This effect could be explained by the presence of random background traffic resulting in a higher jitter value for experiments 1-3 and a lower one  for experiment 4.
These results, however, might vary and will be the subject of a separate study where we will collect data traces for different days of the week and times of the day to refine the results of our analysis.

![ts_df_vs_experiment_no_outliers](img/ts_df_vs_experiment_no_outliers.png)

Fig. 4.3.12a: Time-Stamped Delay Factor (TS-DF) in experiments 1 to 3 (left to right); extreme values omitted.

![ts_df_vs_latency_no_outliers](img/ts_df_vs_latency_no_outliers.png)

Fig. 4.3.12b: Time-Stamped Delay Factor (TS-DF) vs latency in experiments 1 to 3 (left to right); extreme values omitted.

The order/value of TS-DF jitter obtained for the tunnelling SRT over QUIC data traces is much lower than what was obtained when streaming via QUIC datagrams. As can be seen from graph 4.3.14a, the average TS-DF  value observed during experiment 4 is 0.32 ms, while the median equals 0.14 ms. For QUIC datagrams, we have an average of 31.84 ms with median of 21.08 ms, respectively (see Fig. 4.3.12a, experiment 4).

(TODO: elaborate on what's the SRT jitter - after de-jittering is done by SRT)

![ts_df_vs_experiment_squic](img/ts_df_vs_experiment_squic.png)

Fig. 4.3.13a: Time-Stamped Delay Factor (TS-DF) in each experiment when tunnelling SRT over QUIC.

![ts_df_vs_latency_squic](img/ts_df_vs_latency_squic.png)

Fig. 4.3.13b: Time-Stamped Delay Factor (TS-DF) vs latency when tunnelling SRT over QUIC in experiments 1 to 3 (left to the right).

The spikes in the TS-DF signal are also significantly lower when tunneling SRT over QUIC datagrams. In experiment 4, the maximum observed value when tunneling SRT over QUIC is 32.15 ms, while for streaming via QUIC we have 307.32 ms. See figures 4.13.11a and 4.3.13a. 

![ts_df_vs_experiment_squic_no_outliers](img/ts_df_vs_experiment_squic_no_outliers.png)

Fig. 4.3.14a: Time-Stamped Delay Factor (TS-DF) in each experiment when tunnelling SRT over QUIC; extreme values omitted..

![ts_df_vs_latency_squic_no_outliers](img/ts_df_vs_latency_squic_no_outliers.png)

Fig. 4.3.14b: Time-Stamped Delay Factor (TS-DF) vs latency when tunnelling SRT over QUIC in experiments 1 to 3 (left to the right); extreme values omitted.

When considering the results of these tests, it's important to note that experiments 1-3 should be treated and analyzed separately as the SRT latency value varies from 400 to 800 ms. The 4th experiment has the same parameters as the 3rd one (SRT latency = 800ms). However, the duration is extended to 1 hour.

TODO: *last part, discuss with Maxim*
- *SQUIC: The less the latency, the higher the spikes in jitter (from 6 ms to 40 ms in maximum). Discuss the reason for that. Both latency 600 and 800 isn't enough. -> This leads to the higher average value. When latency = 800, spikes up to 10 ms - way better + take a look at the experiment 4.* 
- *SQUIC: Notches - significant difference. Why? A metter of probability (randomness). It's interesting why there is difference between 400 & 600 in median. + compare with exp.4.* 
**TS-DF vs Jitter RFC 3550**

TODO: Later, describe this in the body first. Take QUIC -> build box plots TS-DF vs Jitter RFC 3550. + also graphs from plot_metrics.

## Conclusions

TODO: 
- Specifically, for this case, multimedia traffic is highly sensitive to delay, jitter, losses, and available bandwidth variations at the network level. -> SRT helps to compensate. QUIC doesn't provide those options.
- It's important to properly set the latency for SRT protocol.
- ts-df vs jitter rfc 3550, why the first is better


----- Tables -----

Table 1: Percentage of unrecovered and reordered packets per experiment (QUIC).

| Experiment   | Unrecovered Packets, % | Reordered Packets, % |
| ------------ | ---------------------- | -------------------- |
| Experiment 1 | 0.001058               | 0.000019             |
| Experiment 2 | 0.001219               | 0.000035             |
| Experiment 3 | 0.000879               | 0.000016             |
| Experiment 4 | 0.001083               | 0.000019             |

Table 2: Percentage of unrecovered and reordered packets per experiment (SRT over QUIC).

| Experiment   | Unrecovered Packets, % | Reordered Packets, % |
| ------------ | ---------------------- | -------------------- |
| Experiment 1 | 0.000161               | 0                    |
| Experiment 2 | 0.000033               | 0                    |
| Experiment 3 | 0                      | 0                    |
| Experiment 4 | 0                      | 0                    |

Table 3: Time-Stamped Delay Factor (TS-DF) statistics when streaming over QUIC datagrams.

| </br>Statistic         | Experiment</br>1 | Experiment</br>2 | Experiment</br>3 | Experiment</br>4 |
|:---------------------- |:----------------:|:----------------:|:----------------:|:----------------:|
| Min, ms                | 7.35             | 6.05             | 5.94             | 4.70             |
| Max, ms                | 268.32           | 274.36           | 292.19           | 307.32           |
| Mean, ms               | 33.45            | 32.79            | 33.09            | 31.84            |
| Standard Deviation, ms | 37.51            | 36.80            | 36.62            | 36.19            |
| Median, ms             | 22.67            | 22.53            | 22.47            | 21.08            |
    
Table 4: Time-Stamped Delay Factor (TS-DF) statistics when tunnelling SRT over QUIC datagrams.

| </br>Statistic         | Experiment</br>1 | Experiment</br>2 | Experiment</br>3 | Experiment</br>4 |
|:---------------------- |:----------------:|:----------------:|:----------------:|:----------------:|
| Min, ms                | 0.03             | 0.03             | 0.03             | 0.03             |
| Max, ms                | 39.17            | 37.06            | 7.41             | 32.15            |
| Mean, ms               | 0.39             | 0.28             | 0.28             | 0.32             |
| Standard Deviation, ms | 1.63             | 1.34             | 0.65             | 1.09             |
| Median, ms             | 0.15             | 0.11             | 0.13             | 0.14             |
    
