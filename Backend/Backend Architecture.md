````md
⚙️ Backend Architecture

signlingo-backend/
│
├── 📄 main.py
├── 📄 requirements.txt
├── 📄 Dockerfile
├── 📄 docker-compose.yml
├── 📄 alembic.ini
├── 📄 .env
├── 📄 .env.example
├── 📄 pyproject.toml
│
├── 📁 alembic/
│   ├── env.py
│   ├── script.py.mako
│   └── 📁 versions/
│       ├── 001_create_users.py
│       ├── 002_create_content_tables.py
│       ├── 003_create_exercises.py
│       ├── 004_create_progress_tables.py
│       ├── 005_create_gamification_tables.py
│       ├── 006_create_social_tables.py
│       ├── 007_create_multiplayer_tables.py
│       ├── 008_create_story_tables.py
│       ├── 009_create_community_tables.py
│       ├── 010_create_notifications.py
│       ├── 011_create_learned_dictionary.py
│       └── 012_create_speed_sign_tables.py
│
├── 📁 app/
│   ├── 📄 __init__.py
│   ├── 📄 config.py
│   ├── 📄 database.py
│   │
│   ├── 📁 models/
│   │   ├── __init__.py
│   │   ├── user.py
│   │   │   ├── User
│   │   │   ├── UserSession
│   │   │   └── UserFriend
│   │   ├── content.py
│   │   │   ├── SignLanguage
│   │   │   ├── Unit
│   │   │   ├── Lesson
│   │   │   ├── Sign
│   │   │   └── LessonSign
│   │   ├── exercise.py
│   │   │   └── Exercise
│   │   ├── progress.py
│   │   │   ├── UserUnitProgress
│   │   │   ├── UserLessonProgress
│   │   │   ├── UserExerciseAttempt
│   │   │   └── UserSignMastery
│   │   ├── gamification.py
│   │   │   ├── Achievement
│   │   │   ├── UserAchievement
│   │   │   ├── DailyXpLog
│   │   │   ├── HeartTransaction
│   │   │   └── GemTransaction
│   │   ├── social.py
│   │   │   ├── League
│   │   │   ├── LeagueSeason
│   │   │   └── LeagueParticipant
│   │   ├── multiplayer.py
│   │   │   ├── MultiplayerMatch
│   │   │   └── MatchPlayer
│   │   ├── story.py
│   │   │   ├── Story
│   │   │   ├── StoryScene
│   │   │   └── UserStoryProgress
│   │   ├── community.py
│   │   │   ├── CommunityPost
│   │   │   ├── PostComment
│   │   │   └── PostLike
│   │   ├── notification.py
│   │   │   └── Notification
│   │   ├── learned_dictionary.py
│   │   │   ├── UserLearnedSign
│   │   │   └── UserDictionaryStats
│   │   └── speed_sign.py
│   │       ├── SpeedSignLevel
│   │       ├── SpeedSignSession
│   │       ├── SpeedSignRound
│   │       ├── SpeedSignLeaderboard
│   │       └── SpeedSignDailyChallenge
│   │
│   ├── 📁 schemas/
│   │   ├── __init__.py
│   │   ├── user.py
│   │   │   ├── UserCreate
│   │   │   ├── UserUpdate
│   │   │   ├── UserResponse
│   │   │   ├── UserStats
│   │   │   └── UserSettings
│   │   ├── auth.py
│   │   │   ├── LoginRequest
│   │   │   ├── RegisterRequest
│   │   │   ├── TokenResponse
│   │   │   ├── RefreshRequest
│   │   │   └── ForgotPasswordRequest
│   │   ├── content.py
│   │   │   ├── UnitResponse
│   │   │   ├── LessonResponse
│   │   │   ├── SignResponse
│   │   │   └── SignSearchQuery
│   │   ├── exercise.py
│   │   │   ├── ExerciseResponse
│   │   │   ├── ExerciseSubmitRequest
│   │   │   ├── ExerciseResult
│   │   │   └── CameraVerifyRequest
│   │   ├── progress.py
│   │   │   ├── ProgressOverview
│   │   │   ├── WeeklyProgress
│   │   │   ├── MilestoneResponse
│   │   │   └── SignMasteryResponse
│   │   ├── gamification.py
│   │   │   ├── HeartsResponse
│   │   │   ├── GemsResponse
│   │   │   ├── StreakResponse
│   │   │   ├── AchievementResponse
│   │   │   └── LevelProgress
│   │   ├── social.py
│   │   │   ├── FriendRequest
│   │   │   ├── FriendResponse
│   │   │   └── FriendSearchQuery
│   │   ├── multiplayer.py
│   │   │   ├── MatchCreateRequest
│   │   │   ├── MatchJoinRequest
│   │   │   └── MatchResponse
│   │   ├── story.py
│   │   │   ├── StoryResponse
│   │   │   ├── SceneResponse
│   │   │   └── SceneCompleteRequest
│   │   ├── community.py
│   │   │   ├── PostCreate
│   │   │   ├── PostResponse
│   │   │   ├── CommentCreate
│   │   │   └── CommentResponse
│   │   ├── notification.py
│   │   │   ├── NotificationResponse
│   │   │   └── NotificationQuery
│   │   ├── learned_dictionary.py
│   │   │   ├── LearnedSignResponse
│   │   │   ├── LearnedSignQuery
│   │   │   ├── DictionaryStatsResponse
│   │   │   ├── ReviewQueueResponse
│   │   │   ├── ReviewSubmitRequest
│   │   │   ├── PersonalNoteUpdate
│   │   │   └── LearnedSignDetailResponse
│   │   └── speed_sign.py
│   │       ├── SpeedSignLevelResponse
│   │       ├── SpeedSignStartRequest
│   │       ├── SpeedSignStartResponse
│   │       ├── SpeedSignRoundResponse
│   │       ├── SpeedSignAnswerRequest
│   │       ├── SpeedSignAnswerResponse
│   │       ├── SpeedSignCameraAnswerRequest
│   │       ├── SpeedSignLevelCompleteRequest
│   │       ├── SpeedSignLevelCompleteResponse
│   │       ├── SpeedSignCompleteRequest
│   │       ├── SpeedSignCompleteResponse
│   │       ├── SpeedSignSessionResponse
│   │       ├── SpeedSignStatsResponse
│   │       ├── SpeedSignLeaderboardResponse
│   │       └── DailyChallengeResponse
│   │
│   ├── 📁 api/
│   │   ├── __init__.py
│   │   ├── deps.py
│   │   │   ├── get_db (AsyncSession dependency)
│   │   │   ├── get_current_user (JWT decode)
│   │   │   ├── get_current_active_user
│   │   │   ├── get_admin_user
│   │   │   └── get_optional_user
│   │   │
│   │   └── 📁 v1/
│   │       ├── __init__.py
│   │       ├── router.py
│   │       │   └── (aggregates all sub-routers)
│   │       ├── auth.py
│   │       │   ├── POST /register
│   │       │   ├── POST /login
│   │       │   ├── POST /refresh
│   │       │   ├── POST /logout
│   │       │   ├── POST /forgot-password
│   │       │   ├── POST /reset-password
│   │       │   └── POST /google
│   │       ├── users.py
│   │       │   ├── GET  /me
│   │       │   ├── PUT  /me
│   │       │   ├── GET  /me/stats
│   │       │   ├── GET  /{user_id}
│   │       │   ├── PUT  /me/settings
│   │       │   └── POST /me/avatar
│   │       ├── units.py
│   │       │   ├── GET  /
│   │       │   ├── GET  /{unit_id}
│   │       │   └── GET  /{unit_id}/lessons
│   │       ├── lessons.py
│   │       │   ├── GET  /{lesson_id}
│   │       │   ├── GET  /{lesson_id}/exercises
│   │       │   ├── POST /{lesson_id}/start
│   │       │   └── POST /{lesson_id}/complete
│   │       ├── exercises.py
│   │       │   ├── POST /{exercise_id}/submit
│   │       │   └── POST /camera/verify
│   │       ├── signs.py
│   │       │   ├── GET    /
│   │       │   ├── GET    /{sign_id}
│   │       │   ├── GET    /categories
│   │       │   ├── GET    /search
│   │       │   ├── POST   /{sign_id}/bookmark
│   │       │   ├── DELETE /{sign_id}/bookmark
│   │       │   └── GET    /bookmarks
│   │       ├── progress.py
│   │       │   ├── GET /overview
│   │       │   ├── GET /weekly
│   │       │   ├── GET /milestones
│   │       │   ├── GET /recent-lessons
│   │       │   ├── GET /sign-mastery
│   │       │   └── GET /review-queue
│   │       ├── gamification.py
│   │       │   ├── GET  /hearts
│   │       │   ├── POST /hearts/refill
│   │       │   ├── POST /hearts/share
│   │       │   ├── GET  /gems
│   │       │   ├── POST /gems/spend
│   │       │   ├── GET  /streak
│   │       │   └── POST /streak/freeze
│   │       ├── achievements.py
│   │       │   ├── GET /
│   │       │   └── GET /recent
│   │       ├── leaderboard.py
│   │       │   ├── GET /
│   │       │   ├── GET /league
│   │       │   ├── GET /leagues
│   │       │   └── GET /speed-sign
│   │       ├── social.py
│   │       │   ├── GET    /friends
│   │       │   ├── POST   /friends/request
│   │       │   ├── PUT    /friends/request/{request_id}
│   │       │   ├── DELETE /friends/{friend_id}
│   │       │   └── GET    /friends/search
│   │       ├── multiplayer.py
│   │       │   ├── POST /create
│   │       │   ├── POST /join
│   │       │   ├── GET  /matches
│   │       │   └── GET  /{match_id}
│   │       ├── stories.py
│   │       │   ├── GET  /
│   │       │   ├── GET  /{story_id}
│   │       │   ├── POST /{story_id}/start
│   │       │   ├── POST /{story_id}/scene/{scene_id}/complete
│   │       │   └── POST /{story_id}/complete
│   │       ├── community.py
│   │       │   ├── GET    /posts
│   │       │   ├── POST   /posts
│   │       │   ├── GET    /posts/{post_id}
│   │       │   ├── PUT    /posts/{post_id}
│   │       │   ├── DELETE /posts/{post_id}
│   │       │   ├── POST   /posts/{post_id}/like
│   │       │   ├── DELETE /posts/{post_id}/like
│   │       │   └── POST   /posts/{post_id}/comments
│   │       ├── notifications.py
│   │       │   ├── GET /
│   │       │   ├── PUT /{notification_id}/read
│   │       │   └── PUT /read-all
│   │       ├── camera.py
│   │       │   ├── POST /predict
│   │       │   └── POST /predict-sequence
│   │       ├── learned_dictionary.py
│   │       │   ├── GET    /learned
│   │       │   ├── GET    /learned/stats
│   │       │   ├── GET    /learned/review-queue
│   │       │   ├── POST   /learned/review
│   │       │   ├── GET    /learned/{sign_id}
│   │       │   ├── POST   /learned/{sign_id}/star
│   │       │   ├── DELETE /learned/{sign_id}/star
│   │       │   ├── PUT    /learned/{sign_id}/note
│   │       │   ├── DELETE /learned/{sign_id}/note
│   │       │   └── GET    /learned/categories
│   │       ├── speed_sign.py
│   │       │   ├── GET  /levels
│   │       │   ├── POST /start
│   │       │   ├── POST /rounds/{round_id}/answer
│   │       │   ├── POST /rounds/{round_id}/camera-answer
│   │       │   ├── POST /level-complete
│   │       │   ├── POST /complete
│   │       │   ├── GET  /history
│   │       │   ├── GET  /stats
│   │       │   ├── GET  /leaderboard
│   │       │   ├── GET  /daily-challenge
│   │       │   └── POST /daily-challenge/start
│   │       └── admin.py
│   │           ├── POST /signs
│   │           ├── PUT  /signs/{sign_id}
│   │           ├── POST /units
│   │           ├── PUT  /units/{unit_id}
│   │           ├── POST /lessons
│   │           ├── PUT  /lessons/{lesson_id}
│   │           ├── POST /exercises
│   │           ├── PUT  /exercises/{exercise_id}
│   │           ├── POST /achievements
│   │           ├── POST /speed-sign/levels
│   │           ├── PUT  /speed-sign/levels/{level_id}
│   │           ├── POST /speed-sign/daily-challenge
│   │           └── GET  /stats
│   │
│   ├── 📁 services/
│   │   ├── __init__.py
│   │   ├── auth_service.py
│   │   │   ├── register_user()
│   │   │   ├── authenticate_user()
│   │   │   ├── create_tokens()
│   │   │   ├── refresh_tokens()
│   │   │   ├── verify_google_token()
│   │   │   ├── hash_password()
│   │   │   └── verify_password()
│   │   ├── user_service.py
│   │   │   ├── get_profile()
│   │   │   ├── update_profile()
│   │   │   ├── get_stats()
│   │   │   ├── update_settings()
│   │   │   └── upload_avatar()
│   │   ├── lesson_service.py
│   │   │   ├── get_units_with_progress()
│   │   │   ├── get_lesson_with_exercises()
│   │   │   ├── start_lesson_session()
│   │   │   └── complete_lesson()
│   │   ├── exercise_service.py
│   │   │   ├── submit_answer()
│   │   │   ├── verify_camera_answer()
│   │   │   ├── check_answer_correctness()
│   │   │   └── calculate_exercise_xp()
│   │   ├── progress_service.py
│   │   │   ├── get_overview()
│   │   │   ├── get_weekly_progress()
│   │   │   ├── get_milestones()
│   │   │   ├── update_unit_progress()
│   │   │   ├── update_lesson_progress()
│   │   │   └── update_sign_mastery()
│   │   ├── xp_service.py
│   │   │   ├── award_xp()
│   │   │   ├── calculate_level()
│   │   │   ├── xp_for_level()
│   │   │   ├── xp_progress_in_level()
│   │   │   └── check_daily_goal()
│   │   ├── heart_service.py
│   │   │   ├── lose_heart()
│   │   │   ├── refill_hearts()
│   │   │   ├── share_heart()
│   │   │   ├── get_heart_status()
│   │   │   └── refill_with_gems()
│   │   ├── streak_service.py
│   │   │   ├── update_streak()
│   │   │   ├── check_streak_reset()
│   │   │   ├── activate_freeze()
│   │   │   └── get_streak_calendar()
│   │   ├── achievement_service.py
│   │   │   ├── check_achievements()
│   │   │   ├── unlock_achievement()
│   │   │   ├── get_all_achievements()
│   │   │   └── get_recent_achievements()
│   │   ├── leaderboard_service.py
│   │   │   ├── get_weekly_rankings()
│   │   │   ├── get_all_time_rankings()
│   │   │   ├── get_friends_rankings()
│   │   │   ├── get_league_standings()
│   │   │   └── process_promotions_demotions()
│   │   ├── spaced_repetition_service.py
│   │   │   ├── calculate_next_review()
│   │   │   ├── get_review_queue()
│   │   │   └── process_review_quality()
│   │   ├── multiplayer_service.py
│   │   │   ├── create_match()
│   │   │   ├── join_match()
│   │   │   ├── start_match()
│   │   │   ├── submit_round_answer()
│   │   │   └── complete_match()
│   │   ├── story_service.py
│   │   │   ├── get_stories()
│   │   │   ├── start_story()
│   │   │   ├── process_scene()
│   │   │   └── complete_story()
│   │   ├── community_service.py
│   │   │   ├── create_post()
│   │   │   ├── get_feed()
│   │   │   ├── toggle_like()
│   │   │   ├── add_comment()
│   │   │   └── delete_post()
│   │   ├── notification_service.py
│   │   │   ├── send()
│   │   │   ├── get_notifications()
│   │   │   ├── mark_read()
│   │   │   ├── mark_all_read()
│   │   │   └── get_unread_count()
│   │   ├── ml_service.py
│   │   │   ├── predict_static()
│   │   │   ├── predict_dynamic()
│   │   │   ├── load_model()
│   │   │   └── process_landmarks()
│   │   ├── search_service.py
│   │   │   ├── search_signs()
│   │   │   ├── search_signs_fulltext()
│   │   │   └── get_categories()
│   │   ├── learned_dictionary_service.py
│   │   │   ├── add_sign_to_dictionary()
│   │   │   ├── get_learned_signs()
│   │   │   ├── get_learned_sign_detail()
│   │   │   ├── get_stats()
│   │   │   ├── get_review_queue()
│   │   │   ├── review_sign()
│   │   │   ├── toggle_star()
│   │   │   ├── update_note()
│   │   │   ├── get_categories()
│   │   │   └── refresh_stats()
│   │   └── speed_sign_service.py
│   │       ├── start_session()
│   │       ├── generate_rounds()
│   │       ├── submit_answer()
│   │       ├── submit_camera_answer()
│   │       ├── complete_level()
│   │       ├── complete_session()
│   │       ├── get_history()
│   │       ├── get_stats()
│   │       ├── get_leaderboard()
│   │       ├── get_daily_challenge()
│   │       ├── start_daily_challenge()
│   │       ├── calculate_speed_multiplier()
│   │       └── calculate_combo_multiplier()
│   │
│   ├── 📁 core/
│   │   ├── __init__.py
│   │   ├── security.py
│   │   │   ├── create_access_token()
│   │   │   ├── create_refresh_token()
│   │   │   ├── decode_token()
│   │   │   ├── hash_password()
│   │   │   └── verify_password()
│   │   ├── permissions.py
│   │   │   ├── is_admin()
│   │   │   ├── is_premium()
│   │   │   └── can_access_unit()
│   │   ├── exceptions.py
│   │   │   ├── AppException
│   │   │   ├── NotFoundException
│   │   │   ├── UnauthorizedException
│   │   │   ├── ForbiddenException
│   │   │   ├── ValidationException
│   │   │   ├── InsufficientHeartsException
│   │   │   └── InsufficientGemsException
│   │   ├── rate_limiter.py
│   │   │   ├── RateLimiter (SlowAPI wrapper)
│   │   │   └── rate limit configs per endpoint group
│   │   └── constants.py
│   │       ├── XP_REWARDS
│   │       ├── HEART_CONFIG
│   │       ├── GEM_CONFIG
│   │       ├── STREAK_MILESTONES
│   │       ├── ACHIEVEMENT_DEFINITIONS
│   │       ├── SPEED_SIGN_MULTIPLIERS
│   │       ├── SPEED_SIGN_COMBO_THRESHOLDS
│   │       └── MASTERY_LEVELS
│   │
│   ├── 📁 ml/
│   │   ├── __init__.py
│   │   ├── predictor.py
│   │   │   ├── ONNXPredictor (server-side inference)
│   │   │   ├── predict_static_sign()
│   │   │   └── predict_dynamic_sign()
│   │   ├── landmark_processor.py
│   │   │   ├── normalize_landmarks()
│   │   │   ├── center_landmarks()
│   │   │   └── validate_landmarks()
│   │   └── 📁 models/
│   │       ├── asl_static.onnx
│   │       ├── asl_dynamic.onnx
│   │       ├── isl_static.onnx
│   │       └── isl_dynamic.onnx
│   │
│   ├── 📁 websocket/
│   │   ├── __init__.py
│   │   ├── manager.py
│   │   │   ├── ConnectionManager
│   │   │   ├── connect()
│   │   │   ├── disconnect()
│   │   │   ├── send_personal()
│   │   │   └── broadcast()
│   │   ├── multiplayer_ws.py
│   │   │   ├── handle_player_join()
│   │   │   ├── handle_answer_submit()
│   │   │   ├── handle_round_result()
│   │   │   └── handle_match_end()
│   │   ├── notification_ws.py
│   │   │   ├── send_notification()
│   │   │   ├── send_heart_received()
│   │   │   ├── send_friend_request()
│   │   │   └── send_achievement_unlocked()
│   │   └── speed_sign_ws.py
│   │       ├── handle_speed_ready()
│   │       ├── handle_speed_answer()
│   │       ├── handle_speed_round_result()
│   │       ├── handle_speed_level_up()
│   │       └── handle_speed_match_end()
│   │
│   ├── 📁 tasks/
│   │   ├── __init__.py
│   │   ├── celery_app.py
│   │   │   └── Celery app configuration
│   │   ├── heart_refill.py
│   │   │   └── refill_all_users_hearts() [every 30 min]
│   │   ├── streak_check.py
│   │   │   └── check_and_reset_streaks() [daily at midnight]
│   │   ├── league_update.py
│   │   │   └── process_weekly_leagues() [every Monday]
│   │   ├── notification_tasks.py
│   │   │   ├── send_streak_reminders() [daily at 8pm]
│   │   │   └── send_review_reminders() [daily at 9am]
│   │   ├── dictionary_stats_refresh.py
│   │   │   └── refresh_all_user_stats() [every hour]
│   │   └── daily_challenge_generator.py
│   │       └── generate_daily_challenge() [daily at midnight]
│   │
│   └── 📁 utils/
│       ├── __init__.py
│       ├── email.py
│       │   ├── send_welcome_email()
│       │   ├── send_password_reset()
│       │   └── send_notification_email()
│       ├── file_upload.py
│       │   ├── upload_to_s3()
│       │   ├── delete_from_s3()
│       │   └── get_presigned_url()
│       └── helpers.py
│           ├── paginate()
│           ├── calculate_age()
│           └── generate_join_code()
│
├── 📁 tests/
│   ├── conftest.py
│   │   ├── test_db fixture
│   │   ├── test_client fixture
│   │   ├── test_user fixture
│   │   └── auth_headers fixture
│   ├── test_auth.py
│   │   ├── test_register
│   │   ├── test_login
│   │   ├── test_refresh
│   │   ├── test_logout
│   │   └── test_google_oauth
│   ├── test_lessons.py
│   │   ├── test_get_units
│   │   ├── test_start_lesson
│   │   └── test_complete_lesson
│   ├── test_progress.py
│   │   ├── test_overview
│   │   ├── test_weekly
│   │   └── test_sign_mastery
│   ├── test_gamification.py
│   │   ├── test_xp_award
│   │   ├── test_heart_loss
│   │   ├── test_heart_refill
│   │   ├── test_streak_update
│   │   └── test_achievement_unlock
│   ├── test_multiplayer.py
│   │   ├── test_create_match
│   │   ├── test_join_match
│   │   └── test_submit_answer
│   ├── test_learned_dictionary.py
│   │   ├── test_auto_add_on_correct_answer
│   │   ├── test_get_learned_signs
│   │   ├── test_filter_by_mastery
│   │   ├── test_filter_by_category
│   │   ├── test_search_learned
│   │   ├── test_review_sign
│   │   ├── test_spaced_repetition_scheduling
│   │   ├── test_toggle_star
│   │   ├── test_update_note
│   │   ├── test_get_stats
│   │   ├── test_get_review_queue
│   │   └── test_no_duplicate_entries
│   └── test_speed_sign.py
│       ├── test_start_session
│       ├── test_round_generation
│       ├── test_submit_correct_answer
│       ├── test_submit_wrong_answer
│       ├── test_speed_multiplier_calculation
│       ├── test_combo_multiplier_calculation
│       ├── test_combo_reset_on_wrong
│       ├── test_level_progression
│       ├── test_level_fail
│       ├── test_complete_session
│       ├── test_leaderboard_update
│       ├── test_personal_best
│       ├── test_daily_challenge
│       ├── test_camera_answer
│       └── test_xp_gem_rewards
│
├── 📁 scripts/
│   ├── seed_data.py
│   │   └── Seed languages, units, lessons, signs, exercises
│   ├── seed_signs.py
│   │   └── Bulk import signs from JSON with media URLs
│   ├── seed_achievements.py
│   │   └── Seed all achievement definitions
│   ├── seed_stories.py
│   │   └── Seed story content with scenes
│   └── seed_speed_sign_levels.py
│       └── Seed 11+ speed sign levels per language
│
└── 📁 data/
    ├── signs_asl.json
    ├── signs_isl.json
    ├── signs_bsl.json
    ├── achievements.json
    ├── stories.json
    └── speed_sign_levels.json
```
````
