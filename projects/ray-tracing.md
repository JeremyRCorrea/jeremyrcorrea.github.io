---
layout: default
title: Ray Tracing Project
---

# Ray Tracing

A high-performance path tracer with support for global illumination, importance sampling, and advanced materials.

![Ray Traced Scene](../assets/images/raytracing-example.png)

## Features

- **Progressive Rendering**: Real-time progressive refinement
- **Global Illumination**: Path tracing with multiple bounces
- **Material System**: 
  - Lambertian diffuse
  - Specular reflection
  - Dielectric (glass)
  - Emissive materials
- **Importance Sampling**: Cosine-weighted hemisphere sampling
- **Anti-aliasing**: Multi-sample per pixel

## Technical Details

### Ray-Sphere Intersection

The core ray-sphere intersection uses the quadratic formula:
```cpp
bool Sphere::intersect(const Ray& ray, float& t) {
    Vec3 oc = ray.origin - center;
    float a = dot(ray.direction, ray.direction);
    float b = 2.0f * dot(oc, ray.direction);
    float c = dot(oc, oc) - radius * radius;
    float discriminant = b*b - 4*a*c;
    
    if (discriminant < 0) return false;
    
    t = (-b - sqrt(discriminant)) / (2.0f*a);
    return t > 0;
}
```

### Path Tracing Algorithm
```cpp
Color trace(Ray ray, int depth) {
    if (depth <= 0) return Color(0);
    
    HitRecord hit;
    if (scene.intersect(ray, hit)) {
        Ray scattered;
        Color attenuation;
        
        if (hit.material->scatter(ray, hit, attenuation, scattered)) {
            return attenuation * trace(scattered, depth - 1);
        }
        return Color(0);
    }
    
    // Sky gradient
    return skyColor(ray.direction);
}
```

## Results

[Include rendered images here]

## Performance

- Scene: 100,000 triangles
- Resolution: 1920x1080
- Samples per pixel: 1000
- Render time: ~45 seconds (on RTX 3080)

## Source Code

[Link to GitHub repository]

## References

- Peter Shirley's "Ray Tracing in One Weekend" series
- PBRT: Physically Based Rendering