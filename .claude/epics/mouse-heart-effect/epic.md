---
name: mouse-heart-effect
status: backlog
created: 2025-09-15T00:51:44Z
progress: 0%
prd: .claude/prds/mouse-heart-effect.md
github: https://github.com/Jamespptsr/heartbeat/issues/1
---

# Epic: Mouse Heart Effect Tool

## Overview
A native cross-platform application that displays animated emoji-sized red hearts on mouse clicks, activated only when a specific target application is in focus. Built as single-file executables using platform-native APIs for optimal performance and minimal system impact.

## Architecture Decisions

### Platform-Native Development
- **macOS**: Swift/Objective-C with Cocoa framework for optimal performance
- **Windows**: C++/Win32 API for lightweight executable and system integration
- **Rationale**: Native development ensures minimal resource usage, faster startup, and better OS integration compared to cross-platform frameworks

### Event Monitoring Strategy
- **Active Window Detection**: Poll active window every 100ms to determine target app focus
- **Mouse Hook Integration**: Platform-specific low-level mouse hooks only when target app is active
- **Conditional Activation**: Zero CPU overhead when target app is not focused

### Animation Engine
- **macOS**: Core Animation with CALayer for GPU-accelerated rendering
- **Windows**: Direct2D for hardware-accelerated graphics with smooth animations
- **Frame Management**: 60fps target with adaptive frame dropping under load

## Technical Approach

### Core Components

#### 1. Application State Manager
- Target application monitoring (process name/window title matching)
- Global enable/disable state management
- Full-screen detection and automatic disabling
- System tray integration and menu handling

#### 2. Mouse Event Handler
- Platform-specific low-level mouse hook registration
- Click event capture with cursor position
- Event filtering (left-click only, target app active only)
- Concurrency limiting (max 3 simultaneous animations)

#### 3. Animation System
- Heart sprite rendering (emoji-style heart shape)
- Vertical movement animation (1-second duration)
- Fade-out effect (opacity 100% to 0%)
- GPU-accelerated rendering for smooth 60fps
- Automatic cleanup after animation completion

#### 4. Configuration Management
- Simple settings file (JSON format)
- Color customization (hex color values)
- Target application selection
- Auto-start with system boot configuration

### System Integration

#### macOS Implementation
- **Frameworks**: Cocoa, Core Animation, Core Graphics
- **Permissions**: Accessibility API for mouse monitoring
- **Distribution**: Single .app bundle (<5MB)
- **System Tray**: NSStatusItem with menu

#### Windows Implementation
- **APIs**: Win32, Direct2D, User32 for mouse hooks
- **Permissions**: No elevated privileges required
- **Distribution**: Single .exe file (<3MB)
- **System Tray**: Shell_NotifyIcon with context menu

## Implementation Strategy

### Development Phases
1. **Core Engine**: Mouse detection + basic heart animation
2. **Platform Integration**: System tray + settings management
3. **Polish & Optimization**: Performance tuning + edge case handling

### Risk Mitigation
- **Permission Issues**: Clear documentation for required system permissions
- **Performance Impact**: Extensive profiling and optimization during development
- **Cross-Platform Consistency**: Parallel development with shared animation specifications

### Testing Approach
- **Unit Tests**: Core animation logic and state management
- **Integration Tests**: Mouse hook reliability across different applications
- **Performance Tests**: Memory/CPU usage under various load conditions
- **User Acceptance Tests**: Real-world usage scenarios with target applications

## Task Breakdown Preview

High-level task categories that will be created:
- [ ] **Platform Foundation**: Set up native development environment and basic app structure
- [ ] **Mouse Event System**: Implement cross-platform mouse click detection with target app filtering
- [ ] **Animation Engine**: Create smooth 60fps heart animations with proper rendering
- [ ] **System Integration**: Add system tray, settings, and auto-start functionality
- [ ] **Performance Optimization**: Ensure <30MB RAM and <1% CPU usage requirements
- [ ] **Testing & Validation**: Comprehensive testing across platforms and edge cases
- [ ] **Distribution Setup**: Create single-file executables with proper code signing

## Dependencies

### External Dependencies
- **macOS**: Xcode development environment, Apple Developer Program for code signing
- **Windows**: Visual Studio or MinGW, Windows SDK, optional code signing certificate
- **Graphics**: Platform-native APIs (no external graphics libraries required)

### Internal Dependencies
- Heart emoji design specifications and color schemes
- Target application identification methods (process names, window titles)
- Performance benchmarking tools and testing environments

## Success Criteria (Technical)

### Performance Benchmarks
- Memory usage: <30MB during active animation
- CPU usage: <1% during animations, <0.1% when idle
- Animation latency: <50ms from click detection to heart appearance
- Frame rate: Consistent 60fps animation playback

### Quality Gates
- Zero memory leaks during 24-hour continuous operation
- 100% click detection accuracy when target app is active
- Graceful handling of system sleep/wake and graphics driver updates
- Cross-platform behavioral consistency (identical animation timing)

### Acceptance Criteria
- Single-file executables under 10MB each platform
- Works reliably across Windows 10+ and macOS 10.14+
- Automatic disable during full-screen applications
- Maximum 3 concurrent heart animations without performance degradation

## Estimated Effort

### Overall Timeline
- **Total Development**: 3-4 weeks
- **macOS Implementation**: 1.5 weeks
- **Windows Implementation**: 1.5 weeks
- **Testing & Polish**: 1 week (parallel with development)

### Resource Requirements
- **Primary Developer**: 1 developer with native platform experience
- **Testing Resources**: Multiple test machines across OS versions
- **Design Assets**: Simple heart emoji graphics (minimal design work)

### Critical Path Items
1. Mouse hook implementation and permission handling
2. Animation engine with 60fps requirement
3. Cross-platform system tray integration
4. Performance optimization to meet strict resource limits

The implementation focuses on simplicity and performance, leveraging native platform capabilities to create a delightful user experience with minimal system overhead.

## Tasks Created
- [ ] #2 - Platform Foundation - macOS Development Setup (parallel: true)
- [ ] #3 - Platform Foundation - Windows Development Setup (parallel: true)
- [ ] #4 - Core Application Structure (parallel: false)
- [ ] #5 - Mouse Event System - Context-Aware Click Detection (parallel: false)
- [ ] #6 - Animation Engine - Heart Animation Implementation (parallel: false)
- [ ] #7 - System Integration - System Tray and Settings (parallel: true)
- [ ] #8 - Performance Optimization and Testing (parallel: false)
- [ ] #9 - Distribution and Code Signing (parallel: false)

Total tasks: 8
Parallel tasks: 3
Sequential tasks: 5
Estimated total effort: 88-124 hours (11-15.5 days)