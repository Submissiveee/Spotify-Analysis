# Spotify Analysis

## Table of Contents

- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools and Technologies](#tools-and-technologies)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Cleaning and Modeling](#data-cleaning-and-modeling)
- [Data Analysis](#data-analysis)
- [Results and Findings](#results-and-findings)
- [Recommendations For Spotify](#reccomendations-for-spotify)


### Project Overview
---

This spotify analysis project is aim to understand global listening habits and surface actionable insights for playing curation, visualize the listening patterns by day/month and year , also analyze audio-feature trend (danceability,energy, etc.)

![spotify overview](https://github.com/user-attachments/assets/29a1a5ba-7054-42ff-87f3-8a99e9bd7ca9)


### Data Source
---

- Spotify Dataset: Track metadata and streaming counts (publicly available CSV) 
[Onyx Data DataDNA Datatset Challenge - Spotify Most Streamed Songs 2023 Dataset - October 2023.xlsx](https://github.com/user-attachments/files/21170561/Onyx.Data.DataDNA.Datatset.Challenge.-.Spotify.Most.Streamed.Songs.2023.Dataset.-.October.2023.xlsx)

### Tools and Technologies
---

- ChatGPT: Auto-generating song-cover mockups each track
- Python: Merging in external "display picture" URLs
- PowerPoint: Designing canvas background assets
- PowerBI: Data cleaning & modeling (Power Query)
           Report creation (Deneb for custom Vega-Lite visuals, Bravo for dynamic calender table)

### Exploratory Data Analysis
---

1. Average Streams per year: 514.14 million
2. Top Streames song: "Blinding Light" by The Weeknd with 3,703,895,074 streams
3. Lowest live stream: "Que Vuelvas" by Carin Leon & Grupo Frontera with 2,762 streams
4. Energy % per track and other audio features (valence, speechiness, liveness, danceability, acousticness) calculated and visualized over release dates and calender periods.

### Data Cleaning and Modeling
---

- Fixed data errors and missing values in power Query 
- Merged in auto-generated cover-image URLs ( via Python) as a new column
- Buit relationships between tracks, feature, and calender tables (Bravo)

### Data Analysis
---

- Seasonal streaming patterns: Grouped by month, day-of-week.
- Feature trends over time: Valence, danceablility,etc., by release year
- Top vs buttom deciles: Streams distribution comparism

### Deneb Challenge
---

  I Spent almost an hour wrestling with a single Vega-Lite spec in Deneb Field mapping wouldn't align, extra "Track Formatted" fields kept appearing as undefined. In the end, renaming measures, stripping formatting, and switching to explicit field names (Month, Day of Week, _Track) cracked it.

![SharedScreenshot](https://github.com/user-attachments/assets/f7ed8e0e-cd84-4845-b1d9-4c15b90f1846)


```Deneb Final Code
{
  "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
  "data": {
    "name": "dataset"
  },
  "config": {
    "autosize": {
      "type": "fit",
      "contains": "padding"
    },
    "view": {
      "stroke": "transparent"
    }
  },
  "vconcat": [
    {
      "height": 38,
      "width": {"step": 38},
      "mark": {
        "type": "bar",
        "stroke": null,
        "cornerRadiusEnd": 4,
        "tooltip": true,
        "color": {
          "expr": "pbiColor(4)"
        }
      },
      "encoding": {
        "x": {
          "field": "Month",
          "type": "ordinal",
          "sort": [
            "Jan", "Feb", "Mar", "Apr", "May", "Jun",
            "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
          ],
          "axis": null
        },
        "y": {
          "field": "_Track",
          "type": "quantitative",
          "aggregate": "mean",
          "axis": null
        }
      }
    },
    {
      "hconcat": [
        {
          "mark": {
            "type": "rect",
            "tooltip": true,
            "stroke": "white",
            "cornerRadius": 6
          },
          "width":{"step": 38},
          "height":{"step": 38},
          "encoding": {
            "x": {
              "field": "Month",
              "type": "ordinal",
              "sort": [
                "Jan", "Feb", "Mar", "Apr", "May", "Jun",
                "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
              ],
              "axis": {
                "labelAngle": 0,
                "labelColor": {
                  "expr": "pbiColor(7)"
                },
                "title": null,
                "titleColor": {
                  "expr": "pbiColor(7)"
                }
              }
            },
            "y": {
              "field": "Day of Week",
              "type": "ordinal",
              "sort": ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
              "axis": {
                "labelAngle": 0,
                "labelColor": {
                  "expr": "pbiColor(7)"
                },
                "title": null,
                "titleColor": {
                  "expr": "pbiColor(7)"
                }
              }
            },
            "color": {
              "field": "_Track",
              "type": "quantitative",
              "aggregate": "mean",
              "scale": {
                "scheme": "pbiColorLinear"
              },
              "legend": null
            }
          }
        },
        {
          "width": 40,
          "height": {"step": 30},
          "mark": {
            "type": "bar",
            "tooltip": true,
            "cornerRadiusEnd": 4,
            "color": {
              "expr": "pbiColor(4)"
            }
          },
          "encoding": {
            "y": {
              "field": "Day of Week",
              "type": "ordinal",
              "sort": ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
              "axis": null
            },
            "x": {
              "field": "_Track",
              "type": "quantitative",
              "aggregate": "mean",
              "axis": null
            }
          }
        }
      ]
    }
  ]
}
```

### Results and Findings
---

  - Weekly peaks: January, febuary and may show highest average streams.
  - Feature insights: Danceability strongly correlates with higher stream counts also acoustic tracks tend to peak in spring months.
 
### Reccomendations for Spotify
---

1. Feature-bases playlists: Curate "High Danceability" lists in summer months.
2. Weekend campaigns : Promote new release on Saturdays and Sundays.
3. Acoustic spring series: Spotlight acoustic/low-energy tracks in september.
4. Artist engagement: Encourage artists to deploy "Que Vuelvas" style live events strategically and use as adds for unsubscribed users to boost low stream tracks.
