# Summary

The United States government routinely performs radiological search deployments to search for the presence of illicit nuclear materials (like highly enriched uranium and weapons grade plutonium) in a given area. The deployments can be intelligence driven, in support of law enforcement, and planned events such as the Super Bowl, presidential inaugurations or political conventions. In a typical deployment, radiation detection systems carried by human operators or mounted on vehicles move in a clearing pattern through the search area. Search teams rely on radiation detection algorithms, running on these systems in real time, to alert them to the presence of an illicit threat source. The detection and identification of sources is complicated by large variation of natural radiation background throughout a given search area and the potential presence of localized non-threat sources such as patients undergoing treatment with medical isotopes. As a result, detection algorithms must be carefully balanced between missing real sources and reporting too many false alarms. The purpose of this competition is to investigate new methodologies in detecting and identifying nuclear materials in a simplified search mission.
  
For more information on the search mission, see https://www.energy.gov/nnsa/nuclear-incident-response.

# Overview

For this competition, we have used Monte Carlo particle transport models (see scale.ornl.gov) to create thousands of data files resembling typical radiological search data collected on urban streets in a mid-sized U.S. city. Each data file – a run – simulates the pulse trains of gamma-ray detection events (amount of energy deposited in the detector at a given time) received by a standard thallium-doped sodium iodide – NaI(Tl) – detector, specifically a 2”×4”×16” NaI(Tl) detector, driving along several city street blocks in a search vehicle. Gamma-rays produced from radioactive decay have a characteristic energy which can be used to identify the source isotope (https://en.wikipedia.org/wiki/Gamma_ray). All of the runs contain data on radiation-detector interaction events from the natural background sources (roadway, sidewalks, and buildings), and some of the runs also contain data arising from non-natural extraneous radiation sources.

The event data created for this competition is derived from a simplified radiation transport model – a street without parked cars, pedestrians, or other “clutter”, a constant search vehicle speed with no stoplights, and no vehicles surrounding the search vehicle. In fact, the search vehicle itself is not even in the model – the detector is moving down the street by itself, 1 meter off the ground. The energy resolution is 7.5% at 661 kilo-electronvolts (keV).This simple model provides a starting point for comparing detection algorithms at their most basic level.

# Objective

The runs are separated into two sets: a training set for which a file with the correct answers (source type, time at which the detector was closest to the source) is also provided, and a test set for which you, the competitor, will populate and submit an answer file for online scoring. For each run in the test set, you’ll use your detection algorithm on the event data to determine

    1. whether there is a non-natural extraneous source along the path (the detection component), and if so,

    2. what type of source it is (identification) and

    3. at what point the detector was closest to it during the run (location — which in this competition will be reported in seconds from the start of the run).

# Data for Download

    testing.tar.gz - Test set — 15,840 runs (6.8 GB)

    training.tar.gz - Training set — 9,700 runs (7.4 GB)

    trainingAnswers.csv - Training set answers file (128 kB)

    submittedAnswers.csv - Blank test set answers file (155 kB)

    sourceInfo.zip - Energy spectra for sources 1-5 (290 kB) – see discussion below

The test set includes runs that will be used to compute your provisional score and ranking while the competition is live as well as runs that will be used to compute your final score and ranking at the end of the competition. Your provisional score will be based on about 42% of the test set runs, and your final score will be based on the remaining 58% of runs.

# Data Description

For the competition we have generated data from thousands of runs that mimic what would be acquired by a 2”×4”×16” NaI(Tl) detector moving down a simplified street in a mid-sized U.S. city. Each run has been designed so that no source is located within the first 30 seconds of measurements, though the first 30 seconds could include events associated with gamma rays arising from an extraneous source located farther along the street.

In a real urban search scenario, you would be able to see the general layout of the street as the detector moves along. A notional diagram is shown in the figure, though the exact street geometry for each run will be different (and unspecified). Buildings made of different materials (red, beige, and dark gray) occupy most of the space along the main street, with a few asphalt parking areas (white), and grass-covered park areas (green). The main street (also white) has four travel lanes, two in each direction, and several cross streets. Travel lanes are each ten feet wide and there is at least ten feet of sidewalk (gray) between the outer travel lane and the building facades. Detector paths are all within one of the four main traffic lanes – with no lane switching and no turning onto the cross streets.

The set of data for a given run mimics what a detector would see in real time: a list of pairs, each comprising the time at which a gamma ray interacted within the radiation detector and the corresponding energy the gamma ray deposited in the detector. Times are in microseconds (µsec, millionths of a second) since the last event, and the deposited energy is in units of kilo-electronvolts (keV). The time recorded for the first event in each run will be zero. Your algorithm can bin the data into any time and energy structure that you find beneficial.

For each run, several characteristics are fixed a priori:

    1. The lane and direction of travel.

    2. The speed of travel, a constant between 1 and 13.4 meters per second.

    3. The street geometry (e.g., what buildings are present where).

  If a source is present, the following characteristics are also fixed:

    1. The type of source (see below for the list of source types and their SourceID labels).

    2. The strength of the source.

    3. Whether the source is shielded or not.

    4. The location of the source along the street.

Another concept we consider is standoff, the distance of the source from the nearest approach of the detector. Standoff is a function of characteristics 1 (lane of travel) and 7 (location of the source along the street).

Although these 7 characteristics are not revealed in the test set, they do impact the data recorded in each file. For instance, fewer events are recorded if the detector is traveling at a faster speed, and more events are recorded if the source has a higher strength.

Note that the runs in the training set and the runs in the test set are all drawn from the same 7-dimensional input space based on the 7 characteristics above. However, the emphasis of each set varies. Recall that the test set runs are divided into those used to calculate your provisional score while the competition is running (42%) and those used to calculate your final score at the end of the competition (58%). The training runs span the full input space relatively evenly, while the final score test runs emphasize the more challenging regions of the input space. The provisional score test runs lie somewhere in between in their emphasis.

# Source Types

If a source is present in a run, it will be one of six types:

SourceID    Source Type

1        HEU: Highly enriched uranium

2        WGPu: Weapons grade plutonium

3        131I: Iodine, a medical isotope

4        60Co: Cobalt, an industrial isotope

5        99mTc: Technetium, a medical isotope

6        A combination of 99mTc and HEU
 

Enter your prediction of the SourceID in your answers file for each run. If no source is present in a run, the SourceID field for that run in your answers file should be entered as 0. For all runs, there will be no source located within the first 30 seconds of measurements.

Energy spectra for each source type are shown below for a significant quantity of source types 1 and 2 and 1 microcurie (μCi) for source types 3-5. In each figure, the solid curve shows the unshielded spectrum while the dashed curve shows the spectrum with 1 cm of lead shielding. The plots show sources 1 meter away from the center of the detector in a vacuum.

# File Formats

Files for this competition are standard ASCII and comma delimited (CSV format). They include run files that contain the radiation detection event data for each run and answers files that contain the source type and source time for all the runs in either the training or the test set.
Run Files

The events for each run (one instance of a detector traveling down a street) are contained in a run file named with a unique run ID number. Each run file contains a list of radiation detection events, one event per row. Each row gives the time in microseconds since the previous recorded event and the deposited energies of this event in keV. For the first event in each run file, the time since the last event is recorded as 0.

Example Run File:

0,69.0
985,154.7
757,55.5
908,1106.9
1105,356.1
391,116.9
…

In this example, the first row records the first event, which had a deposited energy of 69 keV. The second row describes the second event, which occurred 985 µsec after the first with a deposited energy of 154.7 keV. The third row records an event that occurred 757 µsec after the second event (985 +757 = 1742 µsec since the beginning of the run) with a deposited energy of 55.5 keV. The event descriptions continue row-by-row until the end of the run.

# Answers Files

For the training set, the answers file contains the correct answer for each run file in CSV format. Each row of the training set answers file gives an identifier for the run (RunID), an identifier for the source type (SourceID), and the time in seconds, reported to the nearest 1/100 of a second, at which the detector was closest to the source (SourceTime), sometimes referred to as the time of closest approach. For runs without an extraneous source (background only), SourceID and SourceTime are 0 and 0.00, respectively.

Example Training Set Answers File:

RunID,SourceID,SourceTime
100001,0,0.00
100002,0,0.00
100003,0,0.00
100004,1,65.58
100005,0,0.00
100006,6,36.19
100007,1,44.10
…

In the example training set answers file, the first row is for run number 100001, which, like runs 100002 and 100003, had no extraneous source present (0 for SourceID and 0.00 for SourceTime), while the fourth row after the header is for run number 100004, which had source 1 (HEU) present at a location that was nearest to the detector 65.58 seconds after the start of that run.

# Scoring

Scores range from 0 (worst) to 100 (best) and reflect your success on each of the three components: detection, identification, and location.  

Let B be the base score and p be the unit of possible penalty (arbitrary scale). Your score is set to B points and then it is modified according to these rules:

    For each run that contains a source:
        Detection: If you incorrectly say there’s no source present (false negative), you lose 2p points.
        Identification and location: If you correctly say there’s a source present and the distance D between the predicted location and the correct location is less then the standoff (which is different for each run):
            Identification: If you correctly identify the SourceID, you earn p points.
            Location: You earn points according to how far away you are from the correct location, up to a maximum of p points, following this formula:
                                                      “points earned” = p · cos((π / 2) · (D / standoff))
        Location: If you correctly say there’s a source present but get the location wrong (i.e., not within a given standoff), you lose 2p points (so it is the same as false negative).
    For each run that does not contain a source:
        Detection: If you incorrectly say there’s a source present (false positive), you lose 2p points. Otherwise (true negative), you earn 6p points.

 

The values of B and p are selected so that the minimum possible score is 0 and the maximum possible score is 100.

Since it is critical for a working detector not to have too many false positives, runs containing no sources contribute more to your score than runs with a source.

The provisional leaderboard rankings will be based on your scores for approximately 42% of the runs in the test set. This will be updated each time a competitor submits an answers file during the competition.

The final rankings for prizes will be based on the remaining 58% of runs. The scoring from these runs is not revealed to the competitor until the end of the competition, and so the final rankings may be different from what has been shown on the public leaderboard.