# ODIN — early prototype demo

**Status: early prototype.** This is a one-minute edited demonstration from an early handset test. The responses are historical prototype output and can contain errors; they are not current security guidance or a statement of the system's present capabilities.

[![ODIN early prototype preview](../media/odin-early-prototype.gif)](https://github.com/miketigerblue/miketigerblue/blob/main/media/odin-early-prototype.mp4)

[Video with original audio, volume adjusted (MP4)](https://github.com/miketigerblue/miketigerblue/raw/refs/heads/main/media/odin-early-prototype.mp4) · [Caption file (SRT)](../media/odin-early-prototype.srt)

## What the excerpt shows

A real handset conversation about CVE-2024-3400, followed by a question about CISA KEV and EPSS. ODIN connects a SIP/Asterisk telephony path, a streaming voice bridge and threat-intelligence tools. The implementation and operational documentation are in [asterisk-twilio-pbx](https://github.com/miketigerblue/asterisk-twilio-pbx), including its [runbook](https://github.com/miketigerblue/asterisk-twilio-pbx/blob/main/RUNBOOK.md).

This recording makes the voice interface visible. It does not independently verify which backend tools executed or establish the accuracy of every response.

## Prototype context

The longer test was exploratory: I challenged retrieval, source coverage and tool availability. It also exposed unsupported statements about camera access and uncertain explanations of tool limits. Those behaviours are limitations of this early test.

The CVSS, KEV, EPSS and mitigation statements below are preserved as ODIN's answers in that recording. In particular, the quoted EPSS value is a historical response, not a newly checked score.

## Editing and captions

- The main conversation is a continuous excerpt from approximately **06:23.8–07:16.25** of the supplied handset video.
- Opening and closing cards use a still from the same test. The finished video runs about **60 seconds**.
- Original audio and video remain synchronised in the conversation excerpt. Pauses and speech speed are unchanged.
- Captions identify the speakers and normalise punctuation and technical-name spelling from the automatic transcription. Overlapping speech may not be fully represented.
- Audio level is adjusted. Background logs are obscured and source container metadata is omitted.
- The inline GIF is a silent captioned preview; the MP4 includes audio.

## Transcript of the excerpt

**ODIN:** The CVSS base score is a full 10, so it's extremely severe.

**ODIN:** It's known to have been used in ransomware campaigns, and it's listed in the CISA Known Exploited Vulnerabilities catalog since April 2024.

**ODIN:** The recommended action is to apply mitigations or enable threat prevention IDs from the vendor.

**Mike:** You mentioned CISA, KEV and EPSS. Did you mention them?

**ODIN:** Yes, I mentioned both. This CVE is in the CISA Known Exploited Vulnerabilities, KEV catalog, added back in April 2024.

**ODIN:** The EPSS score is also very high, about 0.943, as of the last check, meaning it's got a very high likelihood of being exploited.

**ODIN:** Both indicators confirm it's a top priority vulnerability. Let me know if you need more context or action steps.

---

This is an independent project built outside my employment.
