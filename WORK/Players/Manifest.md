# Manifest

## What is a Video Manifest?

A video manifest, or manifest file, is a _text-based file_ that contains information about a video or audio stream. This file acts as a roadmap for the media player, providing details on retrieving and playing back the video content. The manifest file typically includes information on the stream’s container format, codecs, bitrates, resolution, and other critical details necessary for proper playback.

This simplifies content delivery by allowing the media player to adapt in real-time based on network conditions and device capabilities for optimal playback performance. It ensures the uninterrupted, smooth, and high-quality display of media by intelligently switching between different video bitrates.

## How are Video Manifests Used?

Primarily, video manifests play a key role in `adaptive bitrate (ABR)` streaming systems such as` MPEG-DASH` or `HLS`. _When a viewer hits ‘play’, the media player fetches the manifest file first. It’s like the player’s guidebook, with all the details about the various media files, formats, bitrates, and any potential alternate audio tracks or subtitles_.

In practice, _the media player routinely checks the viewer’s network conditions and leverages the video manifest to pick the most suitable video chunk or fragment to download next_. For instance, if the viewer’s network deteriorates, the player might download a lower bitrate video fragment to keep up with real-time streaming, preventing irritating buffering issues. On the other hand, if there are significant improvements in the network, the player can switch to a higher bitrate for better video quality.

### What Streaming Protocols Support Adaptive Bitrate Streaming?

There are three popular streaming protocols that support adaptive bitrate streaming:

- `HLS` (HTTP Live Streaming) is the most commonly used protocol for video distribution on mobile devices because it’s supported by all major browsers and devices. It supports a variety of bitrates but requires you to use a different URL for each variant.
- `DASH (Dynamic Adaptive Streaming over HTTP)` is newer than HLS and has recently gained popularity due to its ability to dynamically adapt playback quality based on network conditions or device capabilities. For example, if you’re watching from your phone over Wi-Fi with 4G LTE enabled, DASH will automatically switch from 480p to 1080p resolution without buffering or pausing the stream.
- `IIS Smooth Streaming` is another popular option supported by Microsoft Silverlight and iOS devices. However, it only supports streaming up to 1080p quality at maximum. anche in alcuni box

## Protocols

| media streaming protocol                                                     | From                        | Support By                                     |
| ---------------------------------------------------------------------------- | --------------------------- | ---------------------------------------------- |
| [HLS, or HTTP Live Streaming](https://cloudinary.com/glossary/hls-streaming) | Apple                       | All Browser                                    |
| MPEG-DASH (Dynamic Adaptive Streaming over HTTP)                             | [Dash](https://dashif.org/) | All except Safari (Chrome on IOS support Dash) |

### [HLS](https://www.gumlet.com/learn/hls-streaming/)

> In `HLS` video streaming, manifest files have the extension `m3u8` (tipo file di testo) che descrive come recuperare i chunks. Il file di testo descrive a quale minutaggio corrisponde un determinato chunk, questo è per Safari che configura fairplay come DRM sviluppato da Apple

`HLS` is a media streaming protocol that was initially developed by Apple to stream its content. It works by breaking down the overall stream into small HTTP-based file downloads, each of which downloads one short chunk of an overall potentially unbounded transport stream.
`HLS` streaming stands out for its mobile-friendliness The protocol is compatible with Android, iOS, and mobile web browsers.

#### How Does HLS Streaming Work?

The process of `HLS` streaming can be visualized as a harmonious operation among three major components: `The Media Server` the [`Content Delivery Network`](https://www.gumlet.com/learn/what-is-video-cdn/) (CDN), and the `Client-side Video Player`.

### DASH

> In `DASH` fornisce un file formato `MPD (Media Presentation Description)`; se lo apri sembra un xml, usa anche file di testo .xml per i chunk (che sono file fisici storati sulla macchia server e quindi possono essere storati e salvati localmente) e si usa su sistemi operativi che non sono Apple

`DASH` is an adaptive bitrate streaming technology where a multimedia file is partitioned into one or more segments and delivered to a client using HTTP. A media presentation description (`MPD`) describes segment information (timing, UR, media characteristics like video resolution and bit rate.

## My Notes on Protocols

Chrome può usare sia `m3u8` che `DASH` verificare che il player usa muxjs per usare m3u8 su windows perchè m3u8 è solo player

---
