# Tunnelling SRT over QUIC Datagrams (Part 1)
(??) Candidates for proper title
ABSTRACT: TODO

## 1. Introduction

Plan:
- QUIC supports both streams and datagrams
- QUIC's benefits
- datagrams can be used for real-time data transmission as ...
- we made an experiment and implemented SRT over QUIC and compared it with the QUIC connection only
- latency managemnet, retransmission of packets, etc. is implemented by the SRT protocol as DATAGRAMs are unreliable.
- a note regarding congestion control which wasn't disabled - a matter of separate study though
Some notes:
- The QUIC DATAGRAM extension provides application protocols running over QUIC with a mechanism to send unreliable data while leveraging the security and congestion-control properties of QUIC. {From here https://ietf-wg-masque.github.io/draft-ietf-masque-h3-datagram/draft-ietf-masque-h3-datagram.html#name-introduction}
## 2. Mapping SRT to QUIC Datagrams
DATAGRAM frames are used to transmit application data in an unreliable manner and are structured as follows [TODO: RFC9221]:
~~~
DATAGRAM Frame {
   Type (i) = 0x30..0x31,
   [Length (i)],
   Datagram Data (..)
}
~~~
Fig. xx: DATAGRAM frame format.
where _Length_ (if present) is the length of the _Datagram Data_ field in bytes, and _Datagram Data_ is the bytes of the datagram to be delivered. See [Section 4 of the RFC 9221](https://www.rfc-editor.org/rfc/rfc9221.html#section-4) for the detailed description of the DATAGRAM frame fields.
DATAGRAM frames (like all QUIC frames) must fit completely inside a QUIC packet. Since fragmentation is disabled in QUIC, QUIC packets must fit completely inside a UDP datagram. To tunnel SRT over QUIC datagrams, a single SRT packet should be encapsulated into a single DATAGRAM frame, in particular in the _Datagram Data_ field of a QUIC datagram.
(TODO: picture)
Fig. xx: Mapping SRT to QUIC datagrams.
The schematic structure of an SRT packet is shown in Fig. xx while the detailed one can be found in [Section 3 of the SRT protocol Internet Draft](https://datatracker.ietf.org/doc/html/draft-sharabayko-srt-01#section-3)
TODO: (??) How to make it right and reflect that header is 16 bytes? + improve the scheme in RFC
~~~
SRT Packet {
   Header,
   Payload (..)
}
~~~
Fig. xx: SRT packet structure.
The path MTU (TODO: link) which determines the size of the UDP datagram plays an important role for UDP-based transmission. To determine the actual maximum size of the application data to be transmitted within the SRT packet payload, the overheads of the UDP datagram header, QUIC packet header, QUIC DATAGRAM frame header, and SRT packet header should be substructed from the configured path MTU.
## 3. Proof of Concept Implementation
From the list of various QUIC transport implementations (TODO: link), [`quicly`](https://github.com/h2o/quicly) library by Fastly was selected for the project as it supports both QUIC STREAM and DATAGRAM frames. It has been relatively easy to integrate the library into the [`srt-xtransmit`](https://github.com/maxsharabayko/srt-xtransmit) test application as it is lightweight and it only has a few external dependencies. Additional points have been given to `quicly` for being written in C what is in correspondence with the [main SRT protocol C++03 implementation](https://github.com/Haivision/srt) and for being dependent on OpenSSL 1.1 like the SRT library does.
[`srt-xtransmit`](https://github.com/maxsharabayko/srt-xtransmit) is a testing utility that supports UDP, TCP, SRT, and QUIC transport protocols. At the moment of writing this article, the DATAGRAM frames are only supported for QUIC, STREAM frames are being added. `srt-xtransmit` implements `generate`, `receive`, and `route` commands which allow to simulate live media transmission of a constant or variable bitrate and exclude a pair of encoder and decoder from test setup. This is done not only for the sake of simplicity and faster reproducibility of tests, but also for the purpose of .... (isolation the transport, to evaluate the performance at the transport level only, independent on codecs combination, calculating the metrics for transport level, etc.). (the same system can be used with a combination of a particular encoder, metrics can be calculated)
(TODO: An approach is defined in the following draft written by my co-author and me - https://datatracker.ietf.org/doc/html/draft-sharabayko-moq-metrics-00
This document defines an approach of objectively measuring transmission such metrics like delay, jitter, and loss-related transmission metrics for a QUIC [RFC9000] connection using an artificially generated payload of a specific structure. The measurement is to be carried on an application level to be protocol-independent.)
To be more precise, in the case of sending the data over QUIC datagrams (Fig. xx), `generate` command is used on the client side to generate an [artificial payload of a particular structure](https://datatracker.ietf.org/doc/html/draft-sharabayko-moq-metrics-00#section-3) and send it over a QUIC connection. The packets are received and their payload is parsed at the server side with the `receive` command being used. At the same time, QUIC statistics is being collected on both sides and [metrics](https://datatracker.ietf.org/doc/html/draft-sharabayko-moq-metrics-00#section-4) such as transmission delay, jitter, and others, are being calculated on the server. (TODO: The details of the described approach of objectively measuring transmission delay, etc. can be found in draft ...)
(TODO: Picture with test setup for QUIC, see slide 10 from [here](https://hai365.sharepoint.com/:p:/s/Team-SRT/EUMQnoIWTEBCtMR5kJts1TIBfcQS-mkJMEio2ynCvIUEFw?e=0Aclut))
Fig. xx: ...
When tunnelling SRT over QUIC datagrams (Fig. xx), first a QUIC tunnel (? or connection) is established (or created) between client and server machines with the help of the `route` command. Then routing of SRT packets is being done over the established QUIC connection. As has been mentioned in the introduction, (TODO: SRT serves as an application protocol on top of QUIC, is responsible for ..., while QUIC is providing transport, encryption, etc.)
(TODO: Picture with test setup for SRT over QUIC, see slide 11 from [here](https://hai365.sharepoint.com/:p:/s/Team-SRT/EUMQnoIWTEBCtMR5kJts1TIBfcQS-mkJMEio2ynCvIUEFw?e=0Aclut))
Fig. xx: ...
(TODO: A note regarding CC and the issue in quicly - maybe?)
## 4. Performance Evaluation
### 4.1. Test Setup
Datasets were collected using the test setups shown in figures xx and xx. The transmission was made from the MacBook Pro laptop located in Hamburg, Germany (client side), to the Raspberry Pi (? which model) computer based in Madrid, Spain (server side), over the Wi-Fi network. Detailed characteristics of the machines can be found in (TODO: Table 2 in Appendix A).
(TODO: a little bit about the network charactrestics on a connection)
The [`srt-xtransmit`](https://github.com/maxsharabayko/srt-xtransmit) application on the client generated an artificial payload and sent it over Wi-Fi to the server. The generated constant bitrate (CBR) stream was chosen to be 3 Mbps for both (1) streaming over QUIC datagrams and (2) tunnelling SRT over QUIC datagrams, 6 Mbps in total, to ensure that link capacity would be enough for successful transmission of both streams. It is important to note that streaming was made simultaneously for each experiment to equally capture an effect of possible network congestion or packet loss in both datasets.
The SRT library version under evaluation was [v1.5.0-rc.0](https://github.com/Haivision/srt/releases/tag/v1.5.0-rc.0). The SRT sender (client side) [was configured to measure the input rate internally](https://datatracker.ietf.org/doc/html/draft-sharabayko-srt-01#section-5.1) and the overhead was set to the default value of 25%. This could be achieved by the combination of `maxbw=0&inputbw=0` options passed to the `srt-xtransmit` in URI. See Section 5.1.1 "Configuring Maximum Bandwidth" of the SRT protocol Internet Draft, `INPUTBW_ESTIMATED` mode, as well as [SRTO_MAXBW](https://github.com/Haivision/srt/blob/v1.5.0-rc.0/docs/API/API-socket-options.md#srto_maxbw), [SRTO_INPUTBW](https://github.com/Haivision/srt/blob/v1.5.0-rc.0/docs/API/API-socket-options.md#SRTO_INPUTBW), and [SRTO_OHEADBW](https://github.com/Haivision/srt/blob/v1.5.0-rc.0/docs/API/API-socket-options.md#SRTO_OHEADBW) socket options for details. Default values of [sender and receiver buffers](https://github.com/Haivision/srt/blob/v1.5.0-rc.0/docs/API/configuration-guidelines.md) were used on both client and server sides.
The version of quicly library was fixed at the [5b933b2](https://github.com/h2o/quicly/commit/5b933b2265099d979418c6b4ddb077d5c6a8dafc) commit which introduced an important fix for the QUIC DATAGRAM frames. (TODO: Thanks to Kazuho Oku). (TODO: CC wasn't discabled).
Experiments listed in Table xx below were performed. The duration of the first three experiments was set to 15 minutes while the SRT latency was configured to be 400 ms, 600 ms, and 800 ms, respectively. The 4th experiment was simply a repetition of the 3rd one with data being collected for 1 hour. Note that the SRT latency setting (which defines ...) was applied for the tunnelling SRT over QUIC transmission only.
Table xx: ...
| Experiment   | Description                               |
| ------------ | ----------------------------------------- |
| Experiment 1 | SRT Latency: 400 ms, Duration: 15 minutes |
| Experiment 2 | SRT Latency: 600 ms, Duration: 15 minutes |
| Experiment 3 | SRT Latency: 800 ms, Duration: 15 minutes |
| Experiment 4 | SRT Latency: 800 ms, Duration: 1 hour     |
### 4.2. Performance Metrics
The detailed description of the performance metrics and their calculation can be found in [Section 4 "Performance Metrics"](https://datatracker.ietf.org/doc/html/draft-sharabayko-moq-metrics-00#section-4) of the [mentioned above Internet Draft](https://datatracker.ietf.org/doc/html/draft-sharabayko-moq-metrics-00). During results discussion, we would be in particular interested in the following of them:
- Time-Stamped Delay Factor (TS-DF), later also referred as TS-DF jitter, or simply jitter. Unlike the algorithm defined in [RFC3550], TS-DF one does not use a smoothing factor and therefore gives a very accurate instantaneous result. We prefer using TS-DF metric over RFC3550 jitter in the analysis, however, the calculation of RFC3550 jitter has been done and the graphs are available.
- The number of received, missing, and reordered payloads.
Here is an example snapshot of the `srt-xtransmit` metrics file produced at the receiver side during one of the experiments.
![metrics](img/metrics.png)
Fig. xx: `srt-xtransmit` `.csv` output with metrics.
To calculate the percentage of unrecovered and reordered packets, the following formulas were used:
- Unrecovered Packets (%) = pktLost / (pktReceived + pktLost)
  This metric shows the percentage of lost packets when streaming over QUIC datagrams and percentage of unrecovered (or dropped) by the SRT protocol packets when tunnelling SRT over QUIC.
- Reordered Packets (%) = pktReordered / (pktReceived + pktLost)
As you might guess, the values of received (`pktReceived`), lost (`pktLost`), and reordered (`pktReordered`) packets were taken from one of the `srt-xtransmit` output files. The last row was used as the referenced statistics are accumulated by `srt-xtransmit` during experiment.
As some of the metrics are sensitive to clock drift, it is recommended to synchronize the clocks on both sender and receiver machines before an experiment.
TO BE CONTINUED ...
---- haven't touched yet -----
## References
[[RFC9000]](https://www.rfc-editor.org/info/rfc9000) Iyengar, J., Ed. and M. Thomson, Ed., "QUIC: A UDP-Based Multiplexed and Secure Transport", RFC 9000, DOI 10.17487/RFC9000, May 2021,              <https://www.rfc-editor.org/info/rfc9000>.
[[RFC7323]](https://www.rfc-editor.org/info/rfc7323) Borman, D., Braden, B., Jacobson, V., and R. Scheffenegger, Ed., "TCP Extensions for High Performance", RFC 7323, DOI 10.17487/RFC7323, September 2014, <https://www.rfc-editor.org/info/rfc7323>.
[[QUIC-DATAGRAM]](https://datatracker.ietf.org/doc/draft-ietf-quic-datagram/) Pauly, T., Kinnear, E., and D. Schinazi, "An Unreliable Datagram Extension to QUIC", n.d., https://datatracker.ietf.org/doc/draft-ietf-quic-datagram/.
[[SRTRFC]](https://datatracker.ietf.org/doc/draft-sharabayko-srt/) Sharabayko, M.P., Sharabayko, M.A., Dube, J., Kim, J., and J. Kim, "The SRT Protocol", December 2019, <https://datatracker.ietf.org/doc/draft-sharabayko-srt/>.
TODO: Don't forget about ack-s section.