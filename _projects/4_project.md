---
layout: page
title: Team GeoAwareGPT
description: Bringing Geo-Spatial Context to LLMs at the Bharatiya Antariksh Hackathon 2024
img: assets/img/projects/geoawaregpt/cover_image.jpg
importance: 1
category: fun
related_publications: false
---

Our project was developed for the Bharatiya Antariksh Hackathon 2024 by Team GeoAwareGPT from the Indian Institute of Technology, Madras. The core development team consists of Akshay Govind S, Mahendra Kurup, and Ryan Jacob George.

The central problem our thesis and project tackles is bringing geo-spatial context to Large Language Models (LLMs). Current LLMs are not inherently trained with geo-spatial data in mind. Because most geo-spatial data is temporal in nature, these models are highly susceptible to hallucinations when queried about geographic contexts. Furthermore, training models on large quantities of geo-spatial data results in extremely high training costs. LLMs also lack the innate ability to perform complex domain tasks like image processing and segmentation, and struggle to provide accessibility through voice-to-voice local language experiences.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/geoawaregpt/architecture_diagram.jpg" title="Proposed Architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The proposed architecture diagram showcasing the interactions between the front-end interface, the Query Handler, the LLM Agent, and the Tool Handler interacting with the external environment.
</div>

### System Architecture 

The pipeline begins at the front-end, capturing audio, image, or text inputs. Audio is processed through ASR (Automatic Speech Recognition) and routed to a Query Handler. This handler feeds into the central LLM Agent, which dictates "Tool Calls" to a Tool Handler. 

The Tool Handler is responsible for interfacing with the broader environment: querying Cloud APIs, extracting structured data from a GDAL Database, and processing Unstructured Data. Outputs that require further summarization are passed back to the LLM Agent, which can then deliver a final response through text or TTS (Text-to-Speech).

### Salient Features & Technology Stack

We constructed this system leveraging a mix of proprietary and Free and Open-Source Software (FOSS) alternatives to ensure a robust and flexible tech stack.

| Feature | Technology Used | Open Source Alternative |
| :--- | :--- | :--- |
| **Retrieval Augmented Generation (RAG)** to deal with unstructured text data | OpenAI embeddings + FAISS vector database (FOSS) | Sentence Transformers embedding model + FAISS |
| **Geocoding, geodecoding, POI search, weather, satellite images** | Azure maps API | OSM + PostGIS |
| **Image segmentation** | SAMGeo (SAM - Meta) | Already open source |
| **ASR, TTS** | Azure speech API | Whisper (OpenAI), VoiceCraft (Meta), IndicConformer, IndicTTS (AI4Bharat) |
| **GDAL database** (OSM + provided vector data) | Postgres + PostGIS | Open source data is already being used |

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/geoawaregpt/tool_call_flow.jpg" title="ReAct Agent Flow" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    An example ReAct tool call pipeline translating Hindi speech into an Azure API weather query.
</div>

### Dynamic Tool Calling & Features

To demonstrate the agent's capabilities, we implemented a ReAct text prompting strategy. While we utilized the Gemini API for the agent due to compute resources, the architecture can utilize any open-source LLM. 

When a user asks a query like, "What is the weather in (20,80)?" via Hindi speech, the ASR translates it to English text. The LLM formulates a reasoning thought (e.g., "I should call the weather tool") and outputs a JSON tool call. The structured JSON handler calls the Azure Weather API, retrieves the data, and passes it back to the LLM to output a natural language response.

**Available Flexible Tools:**
* Geocoding and Geo Decoding through fuzzy search.
* Obtaining Points of Interest near to a place.
* Providing Technical Details through Retrieval Augmented Generation.
* Image Segmentation with SAM.
* Getting real time weather data.
* Data retrieval from Geospatial databases using custom tools.
* A Custom Agent to design SQL queries given a Natural Language query (ReAct).
* Map views via OpenStreetMap.

### Unique Selling Points

* **Extensibility:** An extensible framework specifically designed for bringing Geo-Spatial Awareness to LLMs.
* **Complex Integrations:** The ability to seamlessly integrate complex tools like Image Segmentation and Analysis.
* **Data Coupling:** Users have the freedom to couple multiple data-sources to run complex analytics.
* **Format Agnostic:** The solution works with any input or output (text, image or audio) and has the ability to ingest Raster, Vector, and Text inputs. This can be extended to any file format if appropriate tools are added.
* **Self-Improvement:** We employ a Reinforcement Learning Framework for our agents, allowing the system to interact and learn from its own mistakes.

### Looking Forward

Our future scope involves deploying this technology to have real-world impact by bringing geo-spatial data to where it is actually needed. We plan to build a Natural Language interface to Bhuvan/Bhoonidhi and serve geo-spatial data directly through a WhatsApp Voice Notes Channel. Imagine farmers using WhatsApp to get high-quality crop data through easy, voice-to-voice conversations in their own colloquial languages. 

To achieve this, we aim to integrate diverse data sources from various critical domains and fine-tune open-source models on tool calling using synthetic conversation data to build nation-wide, scalable systems.
