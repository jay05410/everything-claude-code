---
# Task State Tracking Template
# This frontmatter tracks the state of any ongoing task or workflow

# Basic Info
task_id: "{uuid}"
task_type: "{type}"  # feature | bugfix | refactor | review | custom
title: "{title}"
description: "{description}"

# Status Tracking
status: "pending"  # pending | in_progress | blocked | completed | cancelled
started_at: null
completed_at: null
blocked_reason: null

# Workflow Progress
workflow: "{workflow_name}"
current_phase: 1
total_phases: "{n}"
current_step: "step-01"
steps_completed: []
checkpoints_passed: []

# Agent Tracking
agents_invoked: []
current_agent: null
session_ids: {}

# Artifacts
artifacts_created: []
files_modified: []
files_created: []

# Context
stack:
  language: "{language}"
  frontend: "{frontend}"
  backend: "{backend}"
  database: "{database}"

# User Preferences
user_skill_level: "intermediate"  # beginner | intermediate | expert
yolo_mode: false

# Metadata
created_at: "{timestamp}"
updated_at: "{timestamp}"
created_by: "{agent}"
---

# {title}

## Overview
{description}

---

<!-- Task content will be appended below -->
