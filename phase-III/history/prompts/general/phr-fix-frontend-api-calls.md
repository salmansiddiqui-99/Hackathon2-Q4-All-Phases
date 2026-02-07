---
id: "0001"
title: "Fix Frontend API Calls Type Error"
stage: "red"
date: "2026-02-07"
surface: "agent"
model: "claude-opus-4-6"
feature: "none"
branch: "main"
user: "unknown"
command: "manual-fix"
labels: ["typescript", "api-client", "frontend", "vercel-deployment"]
links:
  spec: null
  ticket: null
  adr: null
  pr: null
files:
  - "frontend/src/app/tasks/[id]/page.tsx"
tests: []
---

# Fix Frontend API Calls Type Error

## Prompt Text
```
getting this error while deploying frontend folder on vercel "Failed to compile.
./src/app/tasks/[id]/page.tsx:31:60
Type error: Expected 1 arguments, but got 2.
  29 |     try {
  30 |       setLoading(true);
> 31 |       const taskData = await apiClient.getTaskById(userId, taskId);
     |                                                            ^
  32 |       setTask(taskData);
  33 |       setEditTitle(taskData.title);
  34 |       setEditDescription(taskData.description || '');
Next.js build worker exited with code: 1 and signal: null
Error: Command "npm run build" exited with 1"
```

## Response Text
Fixed the TypeScript error by correcting API client method calls in the individual task page, removing the unnecessary userId parameter from all API calls since the API client handles authentication internally.

## Outcome Impact
Resolved the compilation error that was preventing Vercel deployment. The frontend now correctly calls API client methods with the proper number of arguments, matching the method signatures in the API client class.

## Files Summary
- Fixed API client calls in `frontend/src/app/tasks/[id]/page.tsx` by removing the userId parameter from getTaskById, updateTask, toggleTaskCompletion, and deleteTask method calls
- Removed unused userId constant

## Tests Summary
none

## Next Prompts
none

## Reflection Note
The API client handles authentication internally via the stored token, so methods don't need a userId parameter.

## Acceptance Criteria
- [X] Fixed all API client calls to match method signatures
- [X] Removed unused userId constant
- [X] Compilation error resolved
- [X] Ready for Vercel deployment

## Failure Modes Observed
The API client methods were defined to accept only taskId parameters, but the frontend was incorrectly passing both userId and taskId.

## Next Experiment to Improve Prompt Quality
Verify that all API client method calls across the frontend match their method signatures.