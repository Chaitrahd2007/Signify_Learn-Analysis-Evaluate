# 🎨 FRONTEND ARCHITECTURE

```text
signlingo-frontend/
│
├── 📄 next.config.js
├── 📄 tailwind.config.ts
├── 📄 tsconfig.json
├── 📄 package.json
├── 📄 .env.local
├── 📄 middleware.ts
│
├── 📁 public/
│   ├── 📁 icons/
│   ├── 📁 images/
│   │   ├── mascot/
│   │   ├── achievements/
│   │   ├── units/
│   │   ├── avatars/
│   │   └── speed-sign/
│   ├── 📁 sounds/
│   │   ├── correct.mp3
│   │   ├── incorrect.mp3
│   │   ├── levelup.mp3
│   │   ├── achievement.mp3
│   │   ├── streak.mp3
│   │   ├── tick-tock.mp3
│   │   ├── combo.mp3
│   │   ├── time-up.mp3
│   │   └── speed-bonus.mp3
│   ├── 📁 lottie/
│   │   ├── confetti.json
│   │   ├── heart-break.json
│   │   ├── star-burst.json
│   │   ├── loading-hands.json
│   │   ├── speed-lightning.json
│   │   └── combo-fire.json
│   ├── 📁 ml-models/
│   │   ├── asl_static/
│   │   │   ├── model.json
│   │   │   └── weights.bin
│   │   ├── asl_dynamic/
│   │   │   ├── model.json
│   │   │   └── weights.bin
│   │   ├── isl_static/
│   │   └── isl_dynamic/
│   ├── manifest.json
│   └── sw.js
│
├── 📁 src/
│   │
│   ├── 📁 app/
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   ├── loading.tsx
│   │   ├── error.tsx
│   │   ├── not-found.tsx
│   │   │
│   │   ├── 📁 (auth)/
│   │   │   ├── layout.tsx
│   │   │   ├── 📁 login/
│   │   │   │   └── page.tsx
│   │   │   ├── 📁 register/
│   │   │   │   └── page.tsx
│   │   │   ├── 📁 forgot-password/
│   │   │   │   └── page.tsx
│   │   │   └── 📁 onboarding/
│   │   │       └── page.tsx
│   │   │
│   │   ├── 📁 (main)/
│   │   │   ├── layout.tsx
│   │   │   │
│   │   │   ├── 📁 home/
│   │   │   │   └── page.tsx
│   │   │   │
│   │   │   ├── 📁 learn/
│   │   │   │   ├── page.tsx
│   │   │   │   ├── 📁 [unitId]/
│   │   │   │   │   ├── page.tsx
│   │   │   │   │   └── 📁 [lessonId]/
│   │   │   │   │       ├── page.tsx
│   │   │   │   │       └── 📁 complete/
│   │   │   │   │           └── page.tsx
│   │   │   │   └── 📁 review/
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── 📁 practice/
│   │   │   │   ├── page.tsx
│   │   │   │   ├── 📁 camera/
│   │   │   │   │   ├── page.tsx
│   │   │   │   │   └── 📁 [signId]/
│   │   │   │   │       └── page.tsx
│   │   │   │   ├── 📁 quiz/
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── 📁 matching/
│   │   │   │   │   └── page.tsx
│   │   │   │   └── 📁 memory/
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── 📁 games/
│   │   │   │   ├── page.tsx
│   │   │   │   ├── 📁 story/
│   │   │   │   │   ├── page.tsx
│   │   │   │   │   └── 📁 [storyId]/
│   │   │   │   │       └── page.tsx
│   │   │   │   ├── 📁 multiplayer/
│   │   │   │   │   ├── page.tsx
│   │   │   │   │   ├── 📁 lobby/
│   │   │   │   │   │   └── [matchId]/
│   │   │   │   │   │       └── page.tsx
│   │   │   │   │   └── 📁 match/
│   │   │   │   │       └── [matchId]/
│   │   │   │   │           └── page.tsx
│   │   │   │   └── 📁 speed-sign/
│   │   │   │       ├── page.tsx
│   │   │   │       ├── 📁 play/
│   │   │   │       │   └── page.tsx
│   │   │   │       ├── 📁 daily/
│   │   │   │       │   └── page.tsx
│   │   │   │       ├── 📁 results/
│   │   │   │       │   └── [sessionId]/
│   │   │   │       │       └── page.tsx
│   │   │   │       └── 📁 leaderboard/
│   │   │   │           └── page.tsx
│   │   │   │
│   │   │   ├── 📁 dictionary/
│   │   │   │   ├── page.tsx
│   │   │   │   ├── 📁 [signId]/
│   │   │   │   │   └── page.tsx
│   │   │   │   └── 📁 learned/
│   │   │   │       ├── page.tsx
│   │   │   │       ├── 📁 review/
│   │   │   │       │   └── page.tsx
│   │   │   │       └── 📁 [signId]/
│   │   │   │           └── page.tsx
│   │   │   │
│   │   │   ├── 📁 leaderboard/
│   │   │   │   └── page.tsx
│   │   │   │
│   │   │   ├── 📁 community/
│   │   │   │   ├── page.tsx
│   │   │   │   ├── 📁 create/
│   │   │   │   │   └── page.tsx
│   │   │   │   └── 📁 [postId]/
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── 📁 profile/
│   │   │   │   ├── page.tsx
│   │   │   │   ├── 📁 edit/
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── 📁 achievements/
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── 📁 friends/
│   │   │   │   │   └── page.tsx
│   │   │   │   └── 📁 [userId]/
│   │   │   │       └── page.tsx
│   │   │   │
│   │   │   ├── 📁 progress/
│   │   │   │   └── page.tsx
│   │   │   │
│   │   │   ├── 📁 settings/
│   │   │   │   └── page.tsx
│   │   │   │
│   │   │   └── 📁 shop/
│   │   │       └── page.tsx
│   │   │
│   │   └── 📁 api/
│   │       └── 📁 health/
│   │           └── route.ts
│   │
│   ├── 📁 components/
│   │   ├── 📁 ui/
│   │   │   ├── Button.tsx
│   │   │   ├── Card.tsx
│   │   │   ├── Modal.tsx
│   │   │   ├── Input.tsx
│   │   │   ├── Badge.tsx
│   │   │   ├── Avatar.tsx
│   │   │   ├── ProgressBar.tsx
│   │   │   ├── CircularProgress.tsx
│   │   │   ├── Skeleton.tsx
│   │   │   ├── Toast.tsx
│   │   │   ├── Tabs.tsx
│   │   │   ├── Dropdown.tsx
│   │   │   ├── Switch.tsx
│   │   │   ├── Slider.tsx
│   │   │   ├── BottomSheet.tsx
│   │   │   └── CountdownTimer.tsx
│   │   │
│   │   ├── 📁 layout/
│   │   │   ├── BottomNavigation.tsx
│   │   │   ├── TopBar.tsx
│   │   │   ├── Sidebar.tsx
│   │   │   ├── PageHeader.tsx
│   │   │   └── SafeArea.tsx
│   │   │
│   │   ├── 📁 auth/
│   │   │   ├── LoginForm.tsx
│   │   │   ├── RegisterForm.tsx
│   │   │   ├── LanguageSelector.tsx
│   │   │   ├── GoalSelector.tsx
│   │   │   └── OnboardingFlow.tsx
│   │   │
│   │   ├── 📁 home/
│   │   │   ├── PathMap.tsx
│   │   │   ├── ContinueLesson.tsx
│   │   │   ├── RecommendedTopics.tsx
│   │   │   ├── DailyGoalWidget.tsx
│   │   │   ├── StreakWidget.tsx
│   │   │   ├── QuickActions.tsx
│   │   │   ├── DailyChallengeCard.tsx
│   │   │   └── ReviewDueBanner.tsx
│   │   │
│   │   ├── 📁 learn/
│   │   │   ├── UnitCard.tsx
│   │   │   ├── UnitGrid.tsx
│   │   │   ├── LessonNode.tsx
│   │   │   ├── LessonPath.tsx
│   │   │   ├── LessonProgressHeader.tsx
│   │   │   ├── HeartDisplay.tsx
│   │   │   └── LessonComplete.tsx
│   │   │
│   │   ├── 📁 exercises/
│   │   │   ├── ExerciseContainer.tsx
│   │   │   ├── VideoToMeaning.tsx
│   │   │   ├── MeaningToVideo.tsx
│   │   │   ├── MatchingPairs.tsx
│   │   │   ├── TrueFalse.tsx
│   │   │   ├── FillBlank.tsx
│   │   │   ├── OrderSigns.tsx
│   │   │   ├── FingerspellExercise.tsx
│   │   │   ├── AnswerFeedback.tsx
│   │   │   ├── ExplanationPanel.tsx
│   │   │   └── ExerciseTimer.tsx
│   │   │
│   │   ├── 📁 camera/
│   │   │   ├── CameraView.tsx
│   │   │   ├── HandOverlay.tsx
│   │   │   ├── PoseOverlay.tsx
│   │   │   ├── SignDetector.tsx
│   │   │   ├── NativeSignerVideo.tsx
│   │   │   ├── CameraFeedback.tsx
│   │   │   ├── PracticeControls.tsx
│   │   │   └── CameraPermission.tsx
│   │   │
│   │   ├── 📁 dictionary/
│   │   │   ├── SearchBar.tsx
│   │   │   ├── AlphabetFilter.tsx
│   │   │   ├── SignCard.tsx
│   │   │   ├── SignDetail.tsx
│   │   │   ├── SignVideo.tsx
│   │   │   ├── BookmarkButton.tsx
│   │   │   ├── LearnedSignCard.tsx
│   │   │   ├── LearnedSignGrid.tsx
│   │   │   ├── MasteryBadge.tsx
│   │   │   ├── DictionaryStats.tsx
│   │   │   ├── CategoryFilter.tsx
│   │   │   └── PersonalNoteModal.tsx
│   │   │
│   │   ├── 📁 games/
│   │   │   ├── GameCard.tsx
│   │   │   ├── MemoryCardGame.tsx
│   │   │   ├── SpeedMatchGame.tsx
│   │   │   ├── QuizGame.tsx
│   │   │   │
│   │   │   ├── 📁 story/
│   │   │   │   ├── StoryCard.tsx
│   │   │   │   ├── StoryPlayer.tsx
│   │   │   │   ├── DialogueBubble.tsx
│   │   │   │   ├── ChoicePanel.tsx
│   │   │   │   └── StoryComplete.tsx
│   │   │   │
│   │   │   ├── 📁 multiplayer/
│   │   │   │   ├── MatchLobby.tsx
│   │   │   │   ├── PlayerCard.tsx
│   │   │   │   ├── GameBoard.tsx
│   │   │   │   ├── ScoreDisplay.tsx
│   │   │   │   ├── RoundResult.tsx
│   │   │   │   └── MatchResult.tsx
│   │   │   │
│   │   │   └── 📁 speed-sign/
│   │   │       ├── SpeedSignHub.tsx
│   │   │       ├── SpeedSignGame.tsx
│   │   │       ├── SpeedSignRound.tsx
│   │   │       ├── SpeedTimer.tsx
│   │   │       ├── SpeedScoreBoard.tsx
│   │   │       ├── ComboIndicator.tsx
│   │   │       ├── AnswerOptions.tsx
│   │   │       ├── CameraAnswer.tsx
│   │   │       ├── LevelUpBanner.tsx
│   │   │       ├── GameOverScreen.tsx
│   │   │       ├── DailyChallengeCard.tsx
│   │   │       └── SpeedLeaderboard.tsx
│   │   │
│   │   ├── 📁 gamification/
│   │   │   ├── XPBar.tsx
│   │   │   ├── LevelBadge.tsx
│   │   │   ├── StreakCalendar.tsx
│   │   │   ├── HeartSystem.tsx
│   │   │   ├── GemCounter.tsx
│   │   │   ├── AchievementCard.tsx
│   │   │   ├── AchievementUnlock.tsx
│   │   │   ├── DailyGoalComplete.tsx
│   │   │   ├── LevelUpModal.tsx
│   │   │   ├── StreakFreezeModal.tsx
│   │   │   └── RewardAnimation.tsx
│   │   │
│   │   ├── 📁 leaderboard/
│   │   │   ├── LeaderboardList.tsx
│   │   │   ├── LeaderboardItem.tsx
│   │   │   ├── LeagueHeader.tsx
│   │   │   ├── PromotionZone.tsx
│   │   │   └── WeeklyStats.tsx
│   │   │
│   │   ├── 📁 social/
│   │   │   ├── FriendsList.tsx
│   │   │   ├── FriendCard.tsx
│   │   │   ├── HeartShareModal.tsx
│   │   │   ├── AddFriend.tsx
│   │   │   └── FriendActivity.tsx
│   │   │
│   │   ├── 📁 community/
│   │   │   ├── PostCard.tsx
│   │   │   ├── PostForm.tsx
│   │   │   ├── CommentSection.tsx
│   │   │   ├── CommentItem.tsx
│   │   │   └── LikeButton.tsx
│   │   │
│   │   ├── 📁 progress/
│   │   │   ├── OverallProgress.tsx
│   │   │   ├── WeeklyActivityChart.tsx
│   │   │   ├── MilestonesGrid.tsx
│   │   │   ├── RecentLessons.tsx
│   │   │   ├── SignMasteryList.tsx
│   │   │   └── StreakHistory.tsx
│   │   │
│   │   ├── 📁 profile/
│   │   │   ├── ProfileHeader.tsx
│   │   │   ├── StatsGrid.tsx
│   │   │   ├── AchievementShowcase.tsx
│   │   │   ├── EditProfileForm.tsx
│   │   │   └── SettingsPanel.tsx
│   │   │
│   │   └── 📁 shared/
│   │       ├── ErrorBoundary.tsx
│   │       ├── LoadingScreen.tsx
│   │       ├── EmptyState.tsx
│   │       ├── ConfettiEffect.tsx
│   │       ├── LottiePlayer.tsx
│   │       ├── CountdownTimer.tsx
│   │       ├── StarRating.tsx
│   │       └── SoundPlayer.tsx
│   │
│   ├── 📁 hooks/
│   │   ├── useAuth.ts
│   │   ├── useUser.ts
│   │   ├── useUnits.ts
│   │   ├── useLessons.ts
│   │   ├── useExercises.ts
│   │   ├── useProgress.ts
│   │   ├── useHearts.ts
│   │   ├── useGems.ts
│   │   ├── useStreak.ts
│   │   ├── useAchievements.ts
│   │   ├── useLeaderboard.ts
│   │   ├── useFriends.ts
│   │   ├── useDictionary.ts
│   │   ├── useCamera.ts
│   │   ├── useHandDetection.ts
│   │   ├── useSignPrediction.ts
│   │   ├── useMultiplayer.ts
│   │   ├── useStory.ts
│   │   ├── useCommunity.ts
│   │   ├── useNotifications.ts
│   │   ├── useSound.ts
│   │   ├── useHaptic.ts
│   │   ├── useMediaQuery.ts
│   │   ├── useLocalStorage.ts
│   │   ├── useWebSocket.ts
│   │   ├── useLearnedDictionary.ts
│   │   ├── useSpeedSign.ts
│   │   └── usePrecisionTimer.ts
│   │
│   ├── 📁 stores/
│   │   ├── authStore.ts
│   │   ├── userStore.ts
│   │   ├── lessonStore.ts
│   │   ├── exerciseStore.ts
│   │   ├── gameStore.ts
│   │   ├── cameraStore.ts
│   │   ├── settingsStore.ts
│   │   ├── notificationStore.ts
│   │   ├── learnedDictionaryStore.ts
│   │   └── speedSignStore.ts
│   │
│   ├── 📁 lib/
│   │   ├── api.ts
│   │   ├── auth.ts
│   │   ├── constants.ts
│   │   ├── utils.ts
│   │   ├── sounds.ts
│   │   ├── haptics.ts
│   │   │
│   │   ├── 📁 ml/
│   │   │   ├── handDetector.ts
│   │   │   ├── poseDetector.ts
│   │   │   ├── signClassifier.ts
│   │   │   ├── landmarkProcessor.ts
│   │   │   ├── gestureBuffer.ts
│   │   │   └── modelLoader.ts
│   │   │
│   │   └── 📁 game-engines/
│   │       ├── matchingEngine.ts
│   │       ├── memoryEngine.ts
│   │       ├── quizEngine.ts
│   │       ├── storyEngine.ts
│   │       └── speedSignEngine.ts
│   │
│   ├── 📁 services/
│   │   ├── authService.ts
│   │   ├── userService.ts
│   │   ├── unitService.ts
│   │   ├── lessonService.ts
│   │   ├── exerciseService.ts
│   │   ├── progressService.ts
│   │   ├── gamificationService.ts
│   │   ├── leaderboardService.ts
│   │   ├── socialService.ts
│   │   ├── dictionaryService.ts
│   │   ├── multiplayerService.ts
│   │   ├── storyService.ts
│   │   ├── communityService.ts
│   │   ├── notificationService.ts
│   │   ├── learnedDictionaryService.ts
│   │   └── speedSignService.ts
│   │
│   ├── 📁 types/
│   │   ├── user.ts
│   │   ├── content.ts
│   │   ├── exercise.ts
│   │   ├── progress.ts
│   │   ├── gamification.ts
│   │   ├── social.ts
│   │   ├── multiplayer.ts
│   │   ├── story.ts
│   │   ├── community.ts
│   │   ├── ml.ts
│   │   ├── api.ts
│   │   ├── learnedDictionary.ts
│   │   └── speedSign.ts
│   │
│   └── 📁 styles/
│       └── globals.css
│
└── 📁 __tests__/
    ├── components/
    ├── hooks/
    ├── services/
    └── utils/