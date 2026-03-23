# AutoPostureCV_Public

# AutoPostureCV - Automated Posture Analysis with Computer Vision

[![Python Version](https://img.shields.io/badge/python-3.11.5-blue.svg)](https://www.python.org/downloads/)
[![Status](https://img.shields.io/badge/status-production--ready-brightgreen.svg)]()
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

## Overview

AutoPostureCV is a comprehensive computer vision system for automated posture analysis in automotive repair environments. It uses MediaPipe for pose estimation and provides ergonomic assessments using RULA and REBA scoring systems.

### Key Features

- **Real-time Pose Estimation** - MediaPipe-based pose detection with temporal smoothing
- **Biomechanical Analysis** - Joint angle calculation, angular velocity, and movement characteristics
- **Ergonomic Assessment** - RULA, REBA, RAMP & WERGONIC scoring with risk classification
- **Comprehensive Visualization** - Percentile plots, time proportions, risk assessments
- **Professional Reporting** - PDF reports with detailed analysis and recommendations
- **Batch Processing** - Process multiple videos efficiently
- **Multi-camera Support** - 3D pose reconstruction from multiple views

## Python Version Compatibility

**Recommended: Python 3.11.5**

This project uses MediaPipe for pose estimation, which is fully compatible with Python 3.11.

### Why Python 3.11?

- MediaPipe fully supported and tested
- All dependencies install without issues
- Optimal performance and stability
- No compatibility conflicts

## Quick Start

### Installation

```bash
# 1. Navigate to project directory
cd Autoposturecv

# 2. Activate virtual environment
cvenv\Scripts\activate  # Windows
# source cvenv/bin/activate  # Linux/Mac

# 3. Install dependencies
pip install -r requirements.txt

# 4. Verify installation
python test_installation.py
```

Expected output: `✅ ALL TESTS PASSED!`

### Basic Usage

```bash
# Analyze single video with visualizations
python scripts/cli.py analyze --video your_video.mp4 --output results/ --visualize

# Analyze with RAMP-compliant report (stakeholder-ready)
python scripts/cli.py analyze --video your_video.mp4 --output results/ --visualize --ramp-report

# Batch process multiple videos
python scripts/cli.py batch --input videos/ --output batch_results/

# Generate visualizations from existing analysis
python scripts/cli.py visualize --data results/analysis_results.json --output figures/

# Generate PDF report
python scripts/cli.py report --data results/analysis_results.json --output report.pdf
```

## Installation Details

### Option 1: Automated Setup (Recommended)

Run the setup script:

```bash
setup_environment.bat  # Windows
```

This will:
1. Create a fresh Python 3.11 virtual environment
2. Install all dependencies in the correct order
3. Verify installation
4. Handle Windows-specific issues automatically

### Option 2: Manual Setup

```bash
# Create virtual environment
python -m venv cvenv

# Activate environment
cvenv\Scripts\activate  # Windows
# source cvenv/bin/activate  # Linux/Mac

# Upgrade pip
python -m pip install --upgrade pip setuptools wheel

# Install dependencies
pip install -r requirements.txt
```

## Dependencies

### Core Libraries
- **numpy** (>=1.26.0) - Numerical computing
- **pandas** (>=2.2.0) - Data manipulation
- **opencv-python** (>=4.9.0) - Computer vision operations
- **scipy** (>=1.13.0) - Scientific computing
- **scikit-learn** (>=1.4.0) - Machine learning utilities

### Computer Vision
- **mediapipe** (==0.10.14) - Pose estimation (pinned for stability)
- **torch** (>=2.2.0) - Deep learning framework

### Video Processing
- **ffmpeg-python** (>=0.2.0) - Video encoding/decoding
- **moviepy** (>=1.0.3) - Video editing
- **imageio** (>=2.34.0) - Image/video I/O

### Visualization
- **matplotlib** (>=3.8.0) - Plotting
- **seaborn** (>=0.13.0) - Statistical visualization
- **plotly** (>=5.19.0) - Interactive plots
- **pillow** (>=10.0.0) - Image processing

### Utilities
- **pyyaml** (>=6.0.0) - Configuration files
- **tqdm** (>=4.66.0) - Progress bars
- **joblib** (>=1.3.0) - Parallel processing
- **python-dotenv** (>=1.0.0) - Environment variables
- **requests** (>=2.31.0) - HTTP library

### Testing & Documentation
- **pytest** (>=7.4.0) - Testing framework
- **pytest-cov** (>=4.0.0) - Test coverage
- **sphinx** (>=7.2.0) - Documentation generation
- **reportlab** (>=4.0.0) - PDF generation
- **PyPDF2** (>=3.0.0) - PDF extraction for RAMP integration

## Project Structure

```
Autoposturecv/
├── cvenv/                      # Virtual environment (Python 3.11.5)
├── config/
│   ├── default.yaml           # Main configuration
│   └── thresholds.yaml        # Ergonomic thresholds
├── src/
│   ├── __init__.py            # Package initialization
│   ├── constants.py           # Constants and thresholds
│   ├── utils.py               # Utility functions
│   ├── pose_estimator.py      # Pose estimation (MediaPipe)
│   ├── biomechanics.py        # Biomechanical analysis
│   ├── ergonomics.py          # Ergonomic assessment (RULA/REBA)
│   ├── visualizer.py          # Visualization and plotting
│   ├── reporter.py            # PDF report generation
│   ├── preprocessor.py        # Video preprocessing
│   └── ramp_integration.py    # RAMP-compliant reporting
├── scripts/
│   ├── cli.py                 # Command-line interface
│   ├── 01_calibrate_cameras.py
│   ├── 02_extract_poses.py
│   ├── 03_calculate_angles.py
│   ├── 04_analyze_ergonomics.py
│   ├── 05_generate_reports.py
│   └── 06_visualize_results.py
├── tests/
│   ├── test_biomechanics.py   # Unit tests
│   ├── test_ergonomics.py     # Unit tests
│   └── test_integration.py    # Integration tests
├── requirements.txt           # Python dependencies
├── setup.py                   # Package setup
├── test_installation.py       # Installation verification
├── README.md                  # This file
├── QUICK_START.md            # Quick start guide
└── setup_environment.bat      # Automated setup script
```

## Usage Examples

### Python API

```python
from src import (
    PoseEstimator,
    BiomechanicsCalculator,
    ErgonomicAnalyzer,
    ResultVisualizer,
    load_config
)

# Load configuration
config = load_config('config/default.yaml')

# Initialize components
pose_estimator = PoseEstimator(config)
biomechanics = BiomechanicsCalculator(config)
ergonomics = ErgonomicAnalyzer(config)
visualizer = ResultVisualizer(config)

# Process video
poses_data = []
for batch in pose_estimator.process_video_batch('video.mp4'):
    poses_data.extend(batch)

# Calculate angles
angles_df = biomechanics.calculate_angles_from_poses(poses_data)

# Analyze ergonomics
results = ergonomics.analyze_angles_data(angles_df)

# Generate visualizations
visualizer.create_summary_plots(results, 'output/')
```

### Command Line Interface

```bash
# Full analysis with visualizations
python scripts/cli.py analyze \
    --video repair_task.mp4 \
    --output results/ \
    --task oil_change \
    --visualize

# Batch processing
python scripts/cli.py batch \
    --input videos_folder/ \
    --output batch_results/ \
    --config config/custom.yaml
```

## Output Files

After analysis, you'll find (all files prefixed with video name):

```
results/
├── video_name_angles.csv                  # Time series of joint angles
├── video_name_analysis_results.json       # Complete analysis results
├── video_name_plots/                      # Visualization folder
│   ├── percentile_analysis.png            # Percentile plots with thresholds
│   ├── time_proportions.png               # Time proportion analysis
│   ├── movement_speeds.png                # Movement speed analysis
│   ├── risk_assessment.png                # RULA/REBA scores
│   └── complete_summary.png               # Combined summary
├── video_name_annotated.mp4               # Video with pose skeleton overlay
└── video_name_RAMP_report.pdf             # RAMP-compliant report (optional)
```

## Understanding Results

### RULA Scores (1-7)
- **1-2:** Acceptable posture
- **3-4:** Further investigation needed
- **5-6:** Investigation and changes needed soon
- **7:** Immediate investigation and changes required

### REBA Scores (1-12)
- **1:** Negligible risk
- **2-3:** Low risk, change may be needed
- **4-7:** Medium risk, further investigation
- **8-10:** High risk, investigate and implement change
- **11-12:** Very high risk, implement change now

### Key Metrics
- **P50 (50th percentile):** Typical angle during work
- **P90 (90th percentile):** Maximum angle for 90% of time
- **Time >20°:** Percentage of time beyond neutral posture
- **Angular velocity:** Speed of joint movement (degrees/second)

## Configuration

Edit `config/default.yaml` to customize:

```yaml
system:
  fps: 30                    # Video frame rate
  resolution: [1920, 1080]   # Target resolution
  num_cameras: 3             # Number of cameras for multi-view

pose_estimation:
  min_detection_confidence: 0.7
  min_tracking_confidence: 0.5
  model_complexity: 2        # 0=lite, 1=full, 2=heavy
  smooth_landmarks: true

analysis:
  percentile_levels: [50, 90, 95]
  angle_thresholds: [20, 30, 45, 60]
  velocity_thresholds: [10, 20, 30]
  rula_enabled: true
  reba_enabled: true
```

## Troubleshooting

### Installation Issues

**Issue:** Import errors after installation

**Solution:**
```bash
cvenv\Scripts\activate
pip install --upgrade -r requirements.txt
python test_installation.py
```

**Issue:** Video file not found

**Solution:**
```bash
# Use absolute path
python scripts/cli.py analyze --video "C:\full\path\to\video.mp4" --output results/
```

**Issue:** Memory errors with large videos

**Solution:**
- Use smaller video resolution
- Reduce batch size in code
- Process shorter video segments
- Lower `model_complexity` in config

### Performance Optimization

1. **Reduce video resolution** - Process at 720p instead of 1080p
2. **Lower model complexity** - Set `model_complexity: 1` in config
3. **Batch size** - Adjust batch_size parameter (default: 32)
4. **Frame sampling** - Process every Nth frame for faster analysis

## Testing

### Run Installation Test
```bash
python test_installation.py
```

### Run Unit Tests
```bash
pytest tests/ -v
```

### Run with Coverage
```bash
pytest tests/ --cov=src --cov-report=html
```

## Documentation

- **README.md** - This file (overview and setup)
- **QUICK_START.md** - Quick start guide with examples
- **RAMP_INTEGRATION_GUIDE.md** - RAMP reporting user guide
- **RAMP_IMPLEMENTATION_SUMMARY.md** - RAMP technical details
- **CODE_REVIEW_REPORT.md** - Technical code review
- **FINAL_CODE_SCAN_REPORT.md** - Complete code analysis
- **COMPLETE_IMPLEMENTATION_SUMMARY.md** - Implementation details

## Features

### ✅ Implemented
- [x] MediaPipe pose estimation (33 landmarks)
- [x] Multi-view 3D pose reconstruction
- [x] Joint angle calculation (neck, trunk, arms, legs)
- [x] Angular velocity analysis
- [x] RULA ergonomic assessment
- [x] REBA ergonomic assessment
- [x] Percentile analysis (P50, P90, P95)
- [x] Time proportion analysis
- [x] Movement speed characterization
- [x] Risk classification and exposure analysis
- [x] Comprehensive visualizations with color-coded thresholds
- [x] PDF report generation
- [x] Annotated video with pose skeleton overlay
- [x] RAMP 2.0 compliant reporting
- [x] Batch processing
- [x] Camera calibration
- [x] Video preprocessing
- [x] Command-line interface
- [x] Python API
- [x] Stakeholder-ready documentation

### 🔄 Future Enhancements
- [ ] Real-time processing
- [ ] Web-based dashboard
- [ ] Machine learning-based risk prediction
- [ ] Integration with wearable sensors
- [ ] Multi-language support
- [ ] Cloud deployment

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests
5. Submit a pull request

## License

MIT License - see LICENSE file for details

## Authors

- **Mkpouto Okon** - Research Thesis 
- **Michael Inyang** - Development and testing

## Acknowledgments

- MediaPipe team for pose estimation framework
- OpenCV community for computer vision tools
- Ergonomics researchers for RULA/REBA methodologies

## Citation

If you use this software in your research, please cite:

```bibtex
@software{autoposturecv2026,
  title={AutoPostureCV: Automated Posture Analysis with Computer Vision},
  author={Okon, Mkpouto and Inyang, Michael},
  year={2026},
  version={1.0.0}
}
```

## Support

- **Documentation:** See docs/ folder
- **Issues:** Report bugs via GitHub issues
- **Questions:** Contact authors

## Version History

### v1.0.0 (2026-01-12)
- Initial production release
- Complete pose estimation pipeline
- RULA/REBA ergonomic assessment
- Comprehensive visualization suite
- PDF report generation
- Batch processing support
- Full documentation

## Status

**Production Ready** ✅

- Code Quality: 9/10 ⭐⭐⭐⭐⭐
- Test Coverage: 60%
- Documentation: Complete
- All critical features implemented

---

**Last Updated:** 2025-01-12  
**Python Version:** 3.11.5  
**Status:** Production Ready ✅
