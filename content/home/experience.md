---
# An instance of the Experience widget.
# Documentation: https://wowchemy.com/docs/page-builder/
widget: experience

# This file represents a page section.
headless: true

# Order that this section appears on the page.
weight: 40

title: Experience
subtitle:

# Date format for experience
#   Refer to https://wowchemy.com/docs/customization/#date-format
date_format: Jan 2006

# Experiences.
#   Add/remove as many `experience` items below as you like.
#   Required fields are `title`, `company`, and `date_start`.
#   Leave `date_end` empty if it's your current employer.
#   Begin multi-line descriptions with YAML's `|2-` multi-line prefix.
experience:
  - title: "Project Manager"
    company: "LEOS"
    company_url: "http://www.leos-instruments.com/"
    # company_logo: "leos.jpg"
    location: "Rovereto, Italy"
    date_start: "2021-01-07"
    date_end: ""
    description : |
      I am following the production and development of the company's main product, single-frequency lasers. Being LEOS a small company, I am filling roles in both management and engineering.

      - Project management
        - Creation of project requirements, specifications and time-frames
        - Organization of team activities and tasks
        - Exploration and testing of new product features (product management)
      - System production
        - Production of all the mechanical and optical subsystems
        - Assembly and testing of the laser head

  - title: "R&D Engineer"
    company: "LEOS"
    company_url: "http://www.leos-instruments.com/"
    # company_logo: "leos.jpg"
    location: "Rovereto, Italy"
    date_start: "2020-05-25"
    date_end: "2021-01-07"
    description : |
      I followed the development of a new wavemeter capable of measuring a broad wavelength spectrum.

      - Hardware review and debug of new PCB prototypes.
      - Development of embedded algorithms for both MCUs and FPGAs.
      - Data analysis and development of the new measurement algorithm (Python).
    
  - title: "R&D Engineer"
    company: "UpSens"
    company_url: "https://www.upsens.com/"
    # company_logo: "upsens.jpg"
    location: "Trento, Italy"
    date_start: "2019-04-01"
    date_end: "2020-05-15"
    description: |
      I followed the development of 3 research projects, one as *Junior Project Manager*.

      - Hardware and firmware development for multiple IoT devices applied in smart-kitchen appliances.
      - Hardware development for green buildings devices, including the usage of energy harvesting.

  - title: "Internship for MS Thesis Research"
    company: "UpSens"
    company_url: "https://www.upsens.com/"
    # company_logo: "upsens.jpg"
    location: "Trento, Italy"
    date_start: "2018-10-01"
    date_end: "2019-03-01"
    description: |
      I studied the technical and economic feasibility of an innovative multi-sensor device used on smart-kitchen appliances. The results of my work were positive and I continued to develop the product in its following phases.

design:
  columns: '2'
  spacing:
    padding: ["40px", "20px", "40px", "20px" ]
---
