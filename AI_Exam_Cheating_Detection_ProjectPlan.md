# AI-Based Exam Cheating Detection System Using Computer Vision

## 1. Project Title

**AI-Based Exam Cheating Detection System Using Computer Vision**

## 2. Project Overview

The proposed system is an AI-based computer vision application designed to assist in monitoring examinations through CCTV or webcam video. The system analyzes student activities and identifies predefined suspicious behaviors that may indicate possible cheating.

The project focuses on a limited number of observable behaviors such as mobile phone usage, talking, repeated looking toward another student's paper, and unusual head movement. When suspicious behavior is detected, the system can generate an alert and record the event for later review by an invigilator.

> **Important:** The system should be treated as an assistance tool. A detected behavior is only a suspicious event and should not automatically be considered proof of cheating. Final decisions should be made by a human invigilator.

---

## 3. Problem Statement

During examinations, invigilators have to continuously monitor many students at the same time. This can make it difficult to notice every suspicious activity.

A computer-vision-based system can help by continuously analyzing video and highlighting predefined suspicious behaviors. This can reduce the monitoring workload and help invigilators review important events.

---

## 4. Objectives

The main objectives are:

- Detect suspicious examination behaviors using computer vision.
- Monitor students through webcam or CCTV video.
- Detect mobile phone usage.
- Detect talking or interaction between students.
- Detect repeated head/eye movement toward another student.
- Generate an alert when suspicious behavior is detected.
- Record the time and type of detected event.
- Provide a simple dashboard for the invigilator.
- Keep the final decision with the human invigilator.

---

## 5. Proposed System

The proposed system takes live or recorded examination video as input.

The video is divided into frames. The system detects students and relevant objects such as mobile phones. It can also estimate body/head/face landmarks to identify predefined behaviors.

The detected behavior is compared with simple rules and/or an ML model. If a suspicious activity is detected for a specified duration or number of times, the system generates an alert.

### Basic Output

- **Normal Activity**
- **Suspicious Activity Detected**
- **Type of Activity**
- **Time of Detection**
- **Student/Seat Region**

---

## 6. Scope of the Project

To keep the project practical and achievable, the first version will focus on four behaviors:

1. **Mobile phone usage**
2. **Talking**
3. **Repeated looking toward another student**
4. **Unusual/repeated head movement**

The project will not attempt to determine a student's intention or automatically declare that a student has cheated.

---

## 7. System Workflow

```text
Camera / CCTV Video
        |
        v
Video Frame Capture
        |
        v
Student Detection
        |
        +------------------+
        |                  |
        v                  v
Object Detection      Face/Head Analysis
        |                  |
        |                  v
        |             Head/Eye Movement
        |                  |
        v                  |
Mobile Phone          Behavior Analysis
Detection                  |
        |                  |
        +--------+---------+
                 |
                 v
        Suspicious Activity
              Decision
                 |
        +--------+--------+
        |                 |
        v                 v
     Normal          Suspicious
                         |
                         v
                   Alert + Log
                         |
                         v
                 Invigilator Review
```

---

## 8. Main Modules

### Module 1: Video Input

- Capture video from webcam/CCTV.
- Support live camera feed.
- Optionally support pre-recorded examination videos.

### Module 2: Student Detection

- Detect students in each video frame.
- Identify the approximate location of each student.
- Assign a temporary tracking ID where possible.

### Module 3: Mobile Phone Detection

- Detect a visible mobile phone using an object detection model.
- Associate the detection with the nearby student region.
- Generate a suspicious event when phone usage persists or satisfies the selected rule.

### Module 4: Face and Head Analysis

- Detect the face/head region.
- Estimate head orientation or landmarks.
- Monitor repeated sideways/downward movements.

### Module 5: Behavior Analysis

The system combines detections over multiple frames instead of making a decision from a single frame.

Example:

```text
One sideways look
      -> Normal

Repeated sideways looks for a defined duration
      -> Suspicious event
```

### Module 6: Alert System

When a suspicious event crosses the selected threshold:

- Display an alert.
- Show the event type.
- Record the time.
- Optionally save a screenshot/frame.

### Module 7: Monitoring Dashboard

A simple web dashboard can display:

- Live video
- Student/seat regions
- Current status
- Alert count
- Detected activity
- Event history

---

## 9. Technologies Used

### Programming Language

- Python

### Computer Vision

- OpenCV

### Machine Learning / Deep Learning

- TensorFlow / Keras
- A lightweight object detection model such as YOLO

### Face / Pose Analysis

- MediaPipe or another suitable landmark library

### Web Application

- Flask
- HTML
- CSS
- JavaScript

### Development Environment

- VS Code
- Jupyter Notebook / Google Colab, if required

---

## 10. Algorithms and Techniques

### 10.1 Object Detection

An object detection model can be used to identify:

- Students/persons
- Mobile phones

The model returns the object's location and confidence score.

### 10.2 Face/Head Pose Analysis

Head landmarks can be used to estimate whether a student is:

- Looking forward
- Looking left
- Looking right
- Looking downward

### 10.3 Rule-Based Behavior Detection

Instead of trying to train a very complex model for every type of cheating, the project can initially use simple rules.

Example:

```text
IF mobile phone is detected near a student
AND detection continues for N frames
THEN generate suspicious phone-use event.
```

Another example:

```text
IF repeated sideways head movement
AND movement continues above the selected threshold
THEN generate suspicious movement event.
```

This approach makes the project easier to implement and explain.

---

## 11. Dataset

The project can use a combination of:

### A. Public datasets

Use suitable publicly available datasets for:

- Person detection
- Mobile phone/object detection
- Human activity recognition, where appropriate

### B. Custom examination dataset

A small custom dataset can be created using staged examination scenarios.

Example classes:

- Normal sitting
- Looking left
- Looking right
- Talking
- Mobile phone usage
- Looking down

The dataset should be collected with consent and should avoid unnecessary personal information.

---

## 12. Data Collection Plan

A small controlled dataset can be created using volunteers.

### Normal Class

Students:

- Sit normally.
- Write answers.
- Look at their own paper.
- Raise their hand.

### Suspicious Activity Classes

Students can perform staged examples of:

- Looking sideways.
- Talking.
- Holding a phone.
- Repeatedly turning their head.

Record short videos and extract frames for training/testing where needed.

---

## 13. Detection Logic

The system should not classify a student as cheating from a single frame.

Instead, it should use a combination of:

- Detection confidence
- Number of consecutive frames
- Duration
- Repeated behavior
- Student/seat location

### Example

```text
Phone detected for 1 frame
        ↓
Ignore / continue monitoring

Phone detected for several consecutive frames
        ↓
Suspicious phone-use event

Repeated events
        ↓
Increase alert priority

Invigilator
        ↓
Reviews the event
```

---

## 14. Expected Output

The dashboard may show:

```text
EXAM MONITORING SYSTEM

Student/Seat: A12
Status: SUSPICIOUS ACTIVITY
Activity: Mobile Phone Detected
Time: 10:42:18

Student/Seat: B07
Status: NORMAL

Student/Seat: C14
Status: SUSPICIOUS ACTIVITY
Activity: Repeated Sideways Movement
Time: 10:47:05
```

---

## 15. Database / Event Log

Each detected event can be stored as:

| Field | Example |
|---|---|
| Event ID | 001 |
| Seat ID | A12 |
| Activity | Mobile Phone |
| Time | 10:42:18 |
| Confidence | 0.91 |
| Duration | 3 seconds |
| Status | For Review |

A simple SQLite database can be used for the first version.

---

## 16. System Requirements

### Hardware

- Laptop/desktop
- Webcam or CCTV camera
- Minimum 8 GB RAM recommended
- GPU optional for development/training

### Software

- Python 3.x
- OpenCV
- TensorFlow/Keras
- YOLO implementation
- MediaPipe
- Flask
- SQLite
- HTML/CSS/JavaScript

---

## 17. Advantages

- Helps invigilators monitor multiple students.
- Provides continuous video analysis.
- Highlights suspicious events.
- Reduces the need to manually watch every moment.
- Creates an event log for review.
- Can work with webcam/CCTV input.
- Can be developed as a web-based application.

---

## 18. Limitations

- A suspicious action does not necessarily mean cheating.
- Head movement can have legitimate reasons.
- Mobile phones may be partially hidden.
- Poor lighting can affect detection.
- Camera angle can affect accuracy.
- Multiple students may overlap in the camera view.
- False positives and false negatives are possible.
- Performance depends on the quality of the trained model and video.

---

## 19. Ethical and Privacy Considerations

Because the system processes examination video, privacy must be considered.

The project should:

- Use informed consent where appropriate.
- Avoid unnecessary collection of personal information.
- Restrict access to recorded footage.
- Store only necessary event information.
- Follow institutional privacy rules.
- Use the system only for authorized examination monitoring.
- Keep human review in the final decision process.

---

## 20. Testing

The system should be tested using different scenarios.

### Test Case 1

**Input:** Student writing normally  
**Expected:** Normal

### Test Case 2

**Input:** Student repeatedly looks sideways  
**Expected:** Suspicious movement alert

### Test Case 3

**Input:** Student visibly uses a phone  
**Expected:** Mobile phone alert

### Test Case 4

**Input:** Student talks briefly  
**Expected:** Possible suspicious event, depending on the configured duration

### Test Case 5

**Input:** Student moves head once  
**Expected:** No immediate cheating alert

---

## 21. Performance Evaluation

The project can evaluate:

### Object Detection

- Precision
- Recall
- F1-score
- mAP

### Behavior Detection

- Accuracy
- Precision
- Recall
- F1-score

### System Performance

- FPS
- Detection latency
- False alert rate

For a college project, the most important point is to clearly explain how the evaluation dataset was prepared and tested.

---

## 22. Future Scope

Possible future improvements include:

- Better multi-person tracking.
- Improved behavior recognition.
- Audio-based suspicious interaction detection, where legally and institutionally permitted.
- Better handling of crowded examination rooms.
- Real-time notifications to authorized invigilators.
- Automatic generation of examination monitoring reports.
- Integration with existing CCTV systems.
- More robust models for different camera angles and lighting conditions.

---

## 23. Suggested Project Architecture

```text
                +-------------------+
                | Webcam / CCTV     |
                +---------+---------+
                          |
                          v
                +-------------------+
                | Frame Processing  |
                |     OpenCV        |
                +---------+---------+
                          |
             +------------+------------+
             |                         |
             v                         v
   +------------------+       +------------------+
   | Person / Phone   |       | Face / Head     |
   | Detection        |       | Analysis        |
   +--------+---------+       +--------+---------+
            |                          |
            +------------+-------------+
                         |
                         v
                +-------------------+
                | Behavior Analysis |
                +---------+---------+
                          |
                          v
                +-------------------+
                | Alert & Event Log |
                +---------+---------+
                          |
                          v
                +-------------------+
                | Flask Dashboard   |
                +-------------------+
```

---

## 24. Development Plan

### Phase 1: Research

- Study computer vision basics.
- Study object detection.
- Study face/head landmark detection.
- Finalize suspicious behaviors.

### Phase 2: Dataset

- Find suitable public datasets.
- Collect a small controlled custom dataset.
- Label required classes.

### Phase 3: Model Development

- Implement person detection.
- Implement phone detection.
- Implement face/head analysis.
- Develop behavior rules.

### Phase 4: Integration

- Combine detection modules.
- Add event logging.
- Add Flask dashboard.

### Phase 5: Testing

- Test normal activities.
- Test suspicious activities.
- Measure detection performance.
- Reduce unnecessary alerts.

### Phase 6: Documentation

- Prepare report.
- Prepare PPT.
- Prepare system screenshots.
- Prepare demo and viva questions.

---

## 25. Recommended Final-Year Project Version

For a manageable CSE-AIML project, use this scope:

**Core Features**

1. Person detection
2. Mobile phone detection
3. Head/face movement analysis
4. Suspicious behavior rules
5. Alert generation
6. Event logging
7. Flask web dashboard

**Avoid initially:**

- Emotion detection
- Identity/face recognition
- Complex psychological analysis
- Automatic punishment decisions
- Too many cheating categories
- Large-scale cloud deployment

Keeping the scope focused will make development, testing, demonstration, and viva explanation easier.

---

## 26. Proposed Final Title

**AI-Based Exam Cheating Detection System Using Computer Vision**

### Short Title

**AI-Based Exam Cheating Detection**

### One-Line Description

> A computer vision system that monitors examination video and highlights predefined suspicious behaviors such as mobile phone usage, talking, and repeated sideways movement for invigilator review.

---

## 27. Team Details

**Team Name:** JAAI  
**Section:** CSE-AIML 4A  
**Project Type:** Major Project  
**Domain:** Artificial Intelligence and Machine Learning / Computer Vision

---

## 28. Final Project Goal

The goal is not to replace the invigilator. The goal is to build an AI-assisted monitoring system that continuously analyzes examination video and brings potentially suspicious events to the invigilator's attention for human verification.
