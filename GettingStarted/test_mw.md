This article will describe in a {{highlight|step by step approach}} how to handle OpenSDK software till execution of a multimedia use-case on a STi family STMicroelectronics platform.

Close to development profile


== Download software ==
*First check that your host PC setup fits [[pre-requisite]].

*Connect to [https://codex.cro.st.com/projects/oeivi/ OpenSDK project] and request project administrators (by e-mail) to become project member in order to have the right credentials for git access.

*Then register your SSH keys in '''gerrit server''' as described [[STiH4xx Key information#Gerrit|here]].

*Before downloading OpenSDK baseline, make sure that you have Sdk2Readers access. If not, you can request it to [https://codex.cro.st.com/wiki/index.php?pagename=Sdk2Readers&group_id=2570 SDK2 Codex]

*Follow then the instructions '''to get the latest baseline''' in [[STiH4xx Download#Baseline_tags|this article]].
{{Info|Please subscribe to [https://lists.codex.cro.st.com/mailman/listinfo/oeivi-delivery delivery mailing list] to be informed of the latest available tag when available}}

== Build images ==
To build the whole software, follow [[STiH4xx Build#Full_baseline_build|instructions here]].

In this article, you can also check for details & FAQ.

== Board connections ==
Following articles describe with images how to connect board to host PC in order to load, boot and get console:
* [[B2172 "needletail" board]]
* [[B2187A "needletail" board]]
* [[B2020e board]]
* [[B2120 board]]

== Boot system & connect to target ==
[[Boot system through NFS (STMC)]] or<br />
[[How to populate a SD-Card |Populate a SDCARD with prebuilt image]].

== Basic tests ==
* Play a video on [[Wayland]] desktop
First push a video in rootfs on host:
 cp myvideo.mp4 build/tmp-eglibc/work/st-oe-linux-gnueabihf/*-image/1.0.0-r1/rootfs/

Then launch gst-play giving video as input:
 gst-play-1.0 /myvideo.mp4

* Play a video from network (video streaming)
 gst-play-1.0 http://10.129.63.245/Streams/_AVI/AdrenalineRush_X264_MP3_\(1280x720\).avi

== Features & restrictions overview ==
Refer to [[STiH4xx Test report]] to have an overview of supported [[:Category:Use Cases|use cases]] and their current status.

== First steps in architecture ==
'''[[STiH4xx Multimedia architecture overview|Clickable multimedia poster]]''' gives a global view of architecture while allowing to click on blocks to get the detailed view:
{{:STiH4xx Multimedia architecture overview}}

== First steps in debugging ==
After a video playback occured, summary is traced on console:
 delta delta0.17: [ 13:H264] < H264 1280x720 main profile level 5.1 frame wxh=1280x736 dpb=12, 1453 frames decoded, 1452 frames output, max fpsroot@st:~# cat 

you can use kernel debugfs to get more informations on last decoding session:
 cat /sys/kernel/debug/delta/last 
<pre>
[last decoding]
|-[ 13:H264]
  |-[config]
  | |- 1280x720
  | |- max au size=691200
  |
  |-[stream infos]
  | |- H264 1280x720 main profile level 5.1
  | |- frame wxh=1280x736
  | |- dpb=12
  |
  |-[performances]
    |- frames decoded=1453
    |- frames output=1452
    |- avg duration(0.1ms)=64 [min=60, max=338]
    |- avg period(0.1ms)=333 [min=62, max=1304]
    |- avg fps(0.1Hz)=300
    |- max reachable fps=1562
    |- avg bitrate(Kbit/s)=1402 [min=912, max=2704]
</pre>

More information on platform debugging can be found in [[:Category:Debug & Traces]].


[[category:STiH4xx - Baseline | 3]]
[[category:STiH407]]
[[category:STiH416]]
