[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **AI and Machine Learning in IoT**

AI and ML models are increasingly deployed on IoT edge devices for
anomaly detection, predictive maintenance, voice and image recognition,
and autonomous decision-making. This introduces attack surfaces not
covered by traditional IoT testing.

### Model Extraction and Analysis

The IoTA team should attempt to extract ML models from device firmware
or runtime memory. Extracted models may reveal sensitive training data
(through model inversion attacks), proprietary algorithms, or
classification boundaries that can be exploited.

### Adversarial Input Testing

For devices that use ML for sensor data interpretation (image
recognition cameras, voice assistants, predictive maintenance sensors),
the IoTA team should evaluate susceptibility to adversarial inputs
designed to cause misclassification or incorrect decisions. For devices
where AI/ML models make safety-critical decisions (medical devices,
autonomous systems, industrial controllers), the robustness of the
decision pipeline against adversarial conditions should be evaluated
with particular rigor.

### AI-Assisted Analysis

The IoTA team should leverage AI/ML tools to augment their own testing
capabilities, including LLM-assisted firmware reverse engineering (for
decompiled code analysis and vulnerability pattern recognition),
AI-driven protocol fuzzing and test case generation, and automated
threat modeling using the device's architecture documentation and SBOM
as inputs. These tools are force multipliers for human analysts, not
replacements.
