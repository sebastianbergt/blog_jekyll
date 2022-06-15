---
layout: post
title:  "Naive 3D Vector transformations"
date:   2022-06-15 12:29:00 +0000
categories: cpp std matrix vector transformation
---

{% highlight c++ %}#include <array>
#include <cmath>
#include <vector>

struct V3 {
    float x;
    float y;
    float z;
};

struct EulerAngles {
    float yaw;
    float pitch;
    float roll;
};

using Point = V3;
using Points = std::vector<Point>;

using Row = std::array<float, 4>;
using M4x4 = std::array<Row, 4>;  // Matrix is column major, [column][row]

M4x4 identity() {
    M4x4 m{};
    for (std::size_t i{0}; i < m.size(); i++) {
        m[i][i] = 1.0F;
    }
    return m;
}

void setRotation(M4x4& m, float yaw, float pitch, float roll) {
    m[0][0] = std::cos(yaw) * std::cos(pitch);
    m[0][1] = std::cos(yaw) * std::sin(pitch) * std::sin(roll) -
              std::sin(yaw) * std::cos(roll);
    m[0][2] = std::cos(yaw) * std::sin(pitch) * std::cos(roll) +
              std::sin(yaw) * std::sin(roll);
    m[1][0] = std::sin(yaw) * std::cos(pitch);
    m[1][1] = std::sin(yaw) * std::sin(pitch) * std::sin(roll) +
              std::cos(yaw) * std::cos(roll);
    m[1][2] = std::sin(yaw) * std::sin(pitch) * std::cos(roll) -
              std::cos(yaw) * std::sin(roll);
    m[2][0] = -std::sin(pitch);
    m[2][1] = std::cos(pitch) * std::sin(roll);
    m[2][2] = std::cos(pitch) * std::cos(roll);
}

void setRotation(M4x4& m, const EulerAngles& angles) {
    setRotation(m, angles.yaw, angles.pitch, angles.roll);
}

void setTranslation(M4x4& m, float x, float y, float z) {
    m[0][3] = x;
    m[1][3] = y;
    m[2][3] = z;
}

void setTranslation(M4x4& m, const V3 t) { setTranslation(m, t.x, t.y, t.z); }

void setScale(M4x4& m, float scale) { m[3][3] = scale; }

M4x4 init(const EulerAngles& angles, const V3& translation) {
    M4x4 m{};
    setRotation(m, angles.yaw, angles.pitch, angles.roll);
    setTranslation(m, translation.x, translation.y, translation.z);
    setScale(m, 1.0F);
    return m;
}

Point transform(const Point& p, const M4x4& t) {
    Point r{};
    r.x = p.x * t[0][0] + p.y * t[1][0] + p.z * t[2][0] + t[3][0];
    r.y = p.x * t[0][1] + p.y * t[1][1] + p.z * t[2][1] + t[3][1];
    r.z = p.x * t[0][2] + p.y * t[1][2] + p.z * t[2][2] + t[3][2];
    return r;
}

M4x4 operator+(const M4x4& lhs, const M4x4& rhs) {
    M4x4 m{};
    std::size_t size{lhs.size()};
    for (std::size_t col{0}; col < size; col++) {
        for (std::size_t row{0}; row < size; row++) {
            m[col][row] = lhs[col][row] + rhs[col][row];
        }
    }
    return m;
}

M4x4 operator*(const M4x4& lhs, const M4x4& rhs) { // not tested
    M4x4 m{};
    std::size_t size{lhs.size()};
    for (std::size_t col{0}; col < size; col++) {
        for (std::size_t row{0}; row < size; row++) {
            m[col][row] = lhs[col][0] + rhs[0][row];
            for (std::size_t k{1}; k < size; k++) {
                m[col][row] += lhs[col][k] + rhs[k][row];
            }
        }
    }
    return m;
}

#include <iostream>

int main() {
    const Point p{.x = 0.0F, .y = 1.0F, .z = 0.0F};
    std::cout << "Point\n x=" << p.x << "\n y=" << p.y << "\n z=" << p.z
              << "\n";

    M4x4 rot_yaw_90deg{};
    setRotation(rot_yaw_90deg,
                EulerAngles{.yaw = M_PI_2, .pitch = 0.0F, .roll = 0.0F});

    const auto p2 = transform(p, rot_yaw_90deg);
    std::cout << "Point\n x=" << p2.x << "\n y=" << p2.y << "\n z=" << p2.z
              << "\n";
}
{% endhighlight %}

Find the code editable on [Compiler Explorer](https://godbolt.org/z/z6xbrcner)
