╔═══════════════════════════════════════════════════════════════════╗
║                 BACKEND DEVELOPMENT ROADMAP                       ║
╠═══════════════════════════════════════════════════════════════════╣

PHASE 1: FOUNDATION (Weeks 1-3)
──────────────────────────────────────────────────────────────────

Week 1: Project Setup & Auth
├── Initialize FastAPI project structure
├── Configure pyproject.toml + requirements.txt
├── Set up Docker + docker-compose.yml
│   ├── PostgreSQL 15 container
│   ├── Redis 7 container
│   └── Backend container
├── config.py (Pydantic Settings from .env)
├── database.py (async SQLAlchemy engine + session)
├── Alembic setup (alembic.ini + env.py)
├── Core security module
│   ├── JWT token creation (access + refresh)
│   ├── Password hashing (bcrypt)
│   ├── Token decoding + validation
│   └── Token expiration handling
├── User model (SQLAlchemy)
├── UserSession model
├── Auth schemas (Pydantic v2)
├── Auth service
│   ├── register_user()
│   │   ├── Validate email uniqueness
│   │   ├── Validate username uniqueness
│   │   ├── Hash password
│   │   ├── Create user record
│   │   ├── Generate JWT tokens
│   │   └── Send welcome email (Celery task)
│   ├── authenticate_user()
│   │   ├── Verify email + password
│   │   ├── Check is_active
│   │   ├── Generate tokens
│   │   └── Create session record
│   ├── refresh_tokens()
│   │   ├── Validate refresh token
│   │   ├── Check expiration
│   │   ├── Rotate tokens
│   │   └── Update session
│   ├── verify_google_token()
│   │   ├── Verify ID token with Google
│   │   ├── Find or create user
│   │   └── Generate JWT tokens
│   └── logout()
│       └── Invalidate session
├── Auth API endpoints (api/v1/auth.py)
├── Dependencies (api/deps.py)
│   ├── get_db (AsyncSession)
│   ├── get_current_user (JWT decode)
│   └── get_current_active_user
├── Custom exceptions (core/exceptions.py)
├── CORS middleware configuration
├── Rate limiting setup (SlowAPI)
│   └── Auth endpoints: 5 req/min
├── Health check endpoint
├── Initial Alembic migration (001_create_users.py)
├── Tests
│   ├── conftest.py (fixtures: test_db, test_client, test_user)
│   ├── test_register
│   ├── test_login
│   ├── test_refresh
│   ├── test_logout
│   └── test_google_oauth
└── .env.example documentation

Week 2: Content System
├── Models
│   ├── SignLanguage
│   ├── Unit
│   ├── Lesson
│   ├── Sign (with media URLs, ML labels, landmark data)
│   └── LessonSign (join table)
├── Schemas
│   ├── UnitResponse (with progress)
│   ├── LessonResponse (with signs + exercises)
│   ├── SignResponse (with media + mastery)
│   └── SignSearchQuery
├── Migration: 002_create_content_tables.py
├── Services
│   ├── lesson_service.py
│   │   ├── get_units_with_progress()
│   │   │   ├── Fetch all units for language
│   │   │   ├── Join with user_unit_progress
│   │   │   └── Calculate completion percentages
│   │   ├── get_lesson_with_exercises()
│   │   │   ├── Fetch lesson + signs + exercises
│   │   │   └── Order exercises by order_index
│   │   └── get_sign_detail()
│   └── search_service.py
│       ├── search_signs() (category, difficulty, letter)
│       ├── search_signs_fulltext() (PostgreSQL tsvector)
│       └── get_categories()
├── API endpoints
│   ├── units.py (GET /, GET /{id}, GET /{id}/lessons)
│   ├── lessons.py (GET /{id}, GET /{id}/exercises)
│   └── signs.py (GET /, GET /{id}, GET /categories, GET /search)
├── Seed scripts
│   ├── seed_data.py (languages, units, lessons)
│   └── seed_signs.py (signs from JSON with S3 URLs)
├── Seed data files
│   ├── signs_asl.json
│   ├── signs_isl.json
│   └── signs_bsl.json
├── File upload utility (utils/file_upload.py)
│   ├── upload_to_s3()
│   ├── delete_from_s3()
│   └── get_presigned_url()
├── Tests
│   ├── test_get_units
│   ├── test_get_lessons
│   └── test_search_signs
└── Rate limiting: Content endpoints 100 req/min

Week 3: Exercise System
├── Models
│   ├── Exercise (9 types, JSONB options/answers)
│   ├── UserExerciseAttempt
│   └── UserLessonProgress
├── Schemas
│   ├── ExerciseResponse
│   ├── ExerciseSubmitRequest
│   ├── ExerciseResult
│   └── CameraVerifyRequest
├── Migration: 003_create_exercises.py, 004_create_progress_tables.py
├── Services
│   ├── exercise_service.py
│   │   ├── submit_answer()
│   │   │   ├── Validate answer against correct_answer
│   │   │   ├── Record attempt in user_exercise_attempts
│   │   │   ├── Calculate XP earned
│   │   │   ├── Trigger heart loss if wrong
│   │   │   ├── Trigger dictionary auto-add if correct
│   │   │   └── Return result with explanation
│   │   ├── verify_camera_answer()
│   │   │   ├── Receive landmarks + confidence
│   │   │   ├── Compare ml_label with target
│   │   │   ├── Check confidence threshold
│   │   │   ├── Record attempt
│   │   │   └── Return accuracy + feedback
│   │   └── check_answer_correctness()
│   └── lesson_service.py (continued)
│       ├── start_lesson_session()
│       │   ├── Check hearts > 0
│       │   ├── Create session ID (Redis)
│       │   ├── Return ordered exercises
│       │   └── Return hearts remaining
│       └── complete_lesson()
│           ├── Validate session
│           ├── Calculate score + stars
│           ├── Award XP
│           ├── Update lesson progress
│           ├── Update unit progress
│           ├── Check achievements
│           ├── Auto-add signs to dictionary
│           └── Return rewards summary
├── API endpoints
│   ├── exercises.py (POST /{id}/submit, POST /camera/verify)
│   └── lessons.py (POST /{id}/start, POST /{id}/complete)
├── Tests
│   ├── test_submit_correct_answer
│   ├── test_submit_wrong_answer
│   ├── test_camera_verify
│   ├── test_start_lesson
│   └── test_complete_lesson
└── Seed exercises in seed_data.py


PHASE 2: GAMIFICATION (Weeks 4-5)
──────────────────────────────────────────────────────────────────

Week 4: XP, Hearts & Streaks
├── Models
│   ├── DailyXpLog
│   ├── HeartTransaction
│   └── UserSignMastery
├── Schemas
│   ├── HeartsResponse
│   ├── StreakResponse
│   └── LevelProgress
├── Migration: 005_create_gamification_tables.py
├── Services
│   ├── xp_service.py
│   │   ├── award_xp()
│   │   │   ├── Calculate XP based on type + multiplier
│   │   │   ├── Update user.xp_total
│   │   │   ├── Update daily_xp_log
│   │   │   ├── Check daily goal met
│   │   │   ├── Calculate level change
│   │   │   ├── Trigger streak update if goal met
│   │   │   └── Return XP breakdown + level_up flag
│   │   ├── calculate_level() (floor(sqrt(xp/50)) + 1)
│   │   ├── xp_for_level()
│   │   └── xp_progress_in_level()
│   ├── heart_service.py
│   │   ├── lose_heart()
│   │   │   ├── Check premium (unlimited)
│   │   │   ├── Check hearts > 0
│   │   │   ├── Decrement hearts
│   │   │   ├── Log HeartTransaction
│   │   │   └── Return can_continue flag
│   │   ├── refill_hearts()
│   │   │   ├── Check time since last refill
│   │   │   ├── Add 1 heart (up to max)
│   │   │   └── Log transaction
│   │   ├── refill_with_gems()
│   │   │   ├── Check gem balance >= 450
│   │   │   ├── Deduct gems
│   │   │   ├── Set hearts to max
│   │   │   └── Log both transactions
│   │   ├── share_heart()
│   │   │   ├── Verify friendship
│   │   │   ├── Check daily share limit
│   │   │   ├── Check recipient not at max
│   │   │   ├── Transfer heart
│   │   │   └── Send notification
│   │   └── get_heart_status()
│   │       ├── Current hearts
│   │       ├── Next refill time
│   │       └── Time until refill
│   ├── streak_service.py
│   │   ├── update_streak()
│   │   │   ├── Check if daily goal met
│   │   │   ├── Check last streak date
│   │   │   ├── Increment or reset streak
│   │   │   ├── Update longest streak
│   │   │   └── Check streak milestones
│   │   ├── check_streak_reset()
│   │   │   └── Compare last_active with today
│   │   ├── activate_freeze()
│   │   │   ├── Deduct 100 gems
│   │   │   └── Set freeze flag for today
│   │   └── get_streak_calendar()
│   │       └── Last 30 days activity from daily_xp_log
│   └── spaced_repetition_service.py
│       ├── calculate_next_review() (SM-2 algorithm)
│       │   ├── Input: quality (0-5), repetitions, ease_factor, interval
│       │   ├── Update interval based on quality
│       │   ├── Update ease_factor
│       │   └── Return new interval + ease_factor
│       └── process_review_quality()
│           └── Map exercise results to quality 0-5
├── Celery tasks
│   ├── celery_app.py (Redis broker + backend)
│   ├── heart_refill.py
│   │   └── refill_all_users_hearts() [every 30 min]
│   │       ├── Query users with hearts < max_hearts
│   │       ├── Add 1 heart per eligible user
│   │       └── Log transactions in bulk
│   └── streak_check.py
│       └── check_and_reset_streaks() [daily midnight]
│           ├── Find users with streak > 0
│           ├── Check if yesterday's goal was met
│           ├── Check if freeze is active
│           └── Reset streak if no activity + no freeze
├── API endpoints
│   └── gamification.py
│       ├── GET  /hearts
│       ├── POST /hearts/refill
│       ├── POST /hearts/share
│       ├── GET  /streak
│       └── POST /streak/freeze
├── Constants (core/constants.py)
│   ├── XP_REWARDS dict
│   ├── HEART_CONFIG
│   └── STREAK_MILESTONES
├── Tests
│   ├── test_xp_award
│   ├── test_level_calculation
│   ├── test_heart_loss
│   ├── test_heart_refill_time
│   ├── test_heart_refill_gems
│   ├── test_heart_share
│   ├── test_streak_increment
│   ├── test_streak_reset
│   ├── test_streak_freeze
│   └── test_spaced_repetition
└── Rate limiting: Gamification 100 req/min

Week 5: Gems, Achievements & Progress
├── Models
│   ├── Achievement
│   ├── UserAchievement
│   └── GemTransaction
├── Schemas
│   ├── GemsResponse
│   ├── AchievementResponse
│   ├── ProgressOverview
│   └── WeeklyProgress
├── Services
│   ├── achievement_service.py
│   │   ├── check_achievements()
│   │   │   ├── Called after every XP-earning action
│   │   │   ├── Check all unearned achievements
│   │   │   ├── Compare requirement_type + value
│   │   │   ├── Unlock if condition met
│   │   │   ├── Award XP + gems
│   │   │   └── Send notification + WebSocket event
│   │   ├── unlock_achievement()
│   │   ├── get_all_achievements() (with earned status)
│   │   └── get_recent_achievements()
│   ├── progress_service.py
│   │   ├── get_overview()
│   │   │   ├── Overall completion percentage
│   │   │   ├── Units/lessons completed count
│   │   │   ├── Signs learned count
│   │   │   ├── Total XP + level
│   │   │   ├── Dictionary size
│   │   │   └── Speed Sign best
│   │   ├── get_weekly_progress()
│   │   │   ├── Daily XP for last 7 days
│   │   │   ├── Daily lessons count
│   │   │   ├── Daily practice minutes
│   │   │   ├── Goal met flags
│   │   │   └── Daily speed games count
│   │   ├── get_milestones()
│   │   ├── update_unit_progress()
│   │   ├── update_lesson_progress()
│   │   └── update_sign_mastery()
│   └── gem economy integration in xp_service
├── API endpoints
│   ├── gamification.py (GET /gems, POST /gems/spend)
│   ├── achievements.py (GET /, GET /recent)
│   └── progress.py (GET /overview, /weekly, /milestones, etc.)
├── Seed script: seed_achievements.py
├── Data file: achievements.json
├── Tests
│   ├── test_gem_earn
│   ├── test_gem_spend
│   ├── test_achievement_unlock
│   ├── test_achievement_check
│   ├── test_progress_overview
│   └── test_weekly_progress
└── Constants: ACHIEVEMENT_DEFINITIONS, GEM_CONFIG


PHASE 3: ML & CAMERA (Weeks 6-8)
──────────────────────────────────────────────────────────────────

Week 6: ML Infrastructure
├── ML service (services/ml_service.py)
│   ├── load_model()
│   │   ├── Load ONNX model from disk
│   │   ├── Cache in memory (LRU)
│   │   └── Support per-language models
│   ├── predict_static()
│   │   ├── Receive 225-feature landmark vector
│   │   ├── Run ONNX inference
│   │   ├── Apply softmax
│   │   └── Return top-5 predictions + confidence
│   ├── predict_dynamic()
│   │   ├── Receive 30×225 feature matrix
│   │   ├── Run GRU ONNX inference
│   │   └── Return prediction + confidence
│   └── process_landmarks()
│       ├── Normalize coordinates
│       ├── Center on wrist
│       └── Scale to unit range
├── Landmark processor (ml/landmark_processor.py)
│   ├── normalize_landmarks()
│   ├── center_landmarks()
│   └── validate_landmarks()
├── Model files
│   ├── asl_static.onnx
│   ├── asl_dynamic.onnx
│   ├── isl_static.onnx
│   └── isl_dynamic.onnx
├── API endpoints (camera.py)
│   ├── POST /predict (static sign)
│   └── POST /predict-sequence (dynamic sign)
├── Rate limiting: ML endpoints 30 req/min
├── Config
│   ├── ML_MODEL_PATH
│   ├── ML_CONFIDENCE_THRESHOLD (0.7)
│   └── ML_SPEED_CONFIDENCE_THRESHOLD (0.65)
└── Tests
    ├── test_predict_static
    ├── test_predict_dynamic
    └── test_landmark_processing

Week 7-8: Camera Integration & Refinement
├── Camera verify endpoint enhancement
│   ├── Server-side validation of client predictions
│   ├── Confidence threshold comparison
│   └── Fallback prediction if client confidence low
├── Integration with exercise_service
│   ├── Camera exercise type handling
│   ├── XP award for camera practice (+10 XP)
│   └── Heart loss on camera wrong answer
├── Integration with progress_service
│   ├── Update sign mastery on camera practice
│   └── Record confidence scores in attempts
├── Performance optimization
│   ├── Model caching (load once, serve many)
│   ├── Async inference
│   └── Request batching (optional)
└── Tests
    ├── test_camera_exercise_flow
    └── test_server_fallback_prediction


PHASE 4: LEARNED DICTIONARY (Weeks 9-10)
──────────────────────────────────────────────────────────────────

Week 9: Dictionary Backend Core
├── Models
│   ├── UserLearnedSign
│   │   ├── user_id, sign_id (unique together)
│   │   ├── first_learned_at, first_lesson_id, learned_via
│   │   ├── mastery_level (1-5)
│   │   ├── times_reviewed, last_reviewed_at, next_review_at
│   │   ├── ease_factor, interval_days (SM-2)
│   │   ├── correct_count, incorrect_count, total_time_spent
│   │   └── is_starred, is_hidden, personal_note
│   └── UserDictionaryStats
│       ├── user_id, language_id (unique together)
│       ├── total_learned, mastered_count, due_for_review
│       ├── learned_this_week, learned_this_month
│       └── category_counts (JSONB)
├── Schemas
│   ├── LearnedSignResponse
│   ├── LearnedSignQuery (filters + sort + pagination)
│   ├── DictionaryStatsResponse
│   ├── ReviewQueueResponse
│   ├── ReviewSubmitRequest (sign_id, quality 0-5, time_spent)
│   ├── PersonalNoteUpdate
│   └── LearnedSignDetailResponse (sign + learned + history)
├── Migration: 011_create_learned_dictionary.py
├── Service: learned_dictionary_service.py
│   ├── add_sign_to_dictionary()
│   │   ├── Check if sign already exists for user
│   │   ├── If exists: increment correct/incorrect count
│   │   ├── If not: create record with mastery_level=1
│   │   ├── Set first_learned_at, learned_via
│   │   ├── Calculate initial next_review_at (SM-2: +1 day)
│   │   ├── Trigger stats refresh
│   │   └── Return dictionary_added flag
│   │   CALLED FROM:
│   │   ├── exercise_service.submit_answer() (on correct)
│   │   ├── exercise_service.verify_camera_answer() (on correct)
│   │   ├── speed_sign_service.submit_answer() (on correct)
│   │   ├── story_service.process_scene() (on sign practice)
│   │   └── signs API (manual add from global dict)
│   ├── get_learned_signs()
│   │   ├── Filter by: language, category, mastery, starred, due
│   │   ├── Sort by: recent, mastery, alphabetical, reviews
│   │   ├── Search by word/meaning (ILIKE)
│   │   ├── Paginate
│   │   └── Join with signs table for media
│   ├── get_learned_sign_detail()
│   │   ├── Full sign info
│   │   ├── Learned metadata
│   │   └── Practice history from user_exercise_attempts
│   ├── get_stats()
│   │   ├── Return cached stats from user_dictionary_stats
│   │   └── Include mastery distribution
│   ├── get_review_queue()
│   │   ├── Query signs where next_review_at <= NOW()
│   │   ├── Order by next_review_at ASC (most overdue first)
│   │   └── Limit to N signs
│   ├── review_sign()
│   │   ├── Apply SM-2 algorithm
│   │   │   ├── quality >= 3: increase interval
│   │   │   ├── quality < 3: reset interval to 1
│   │   │   └── Update ease_factor
│   │   ├── Update mastery_level
│   │   │   ├── quality >= 4: +1 level (max 5)
│   │   │   └── quality <= 1: -1 level (min 1)
│   │   ├── Update times_reviewed, last_reviewed_at
│   │   ├── Calculate next_review_at
│   │   ├── Award +3 XP
│   │   └── Return updated schedule
│   ├── toggle_star()
│   ├── update_note()
│   ├── get_categories()
│   │   └── Group learned signs by category with counts
│   └── refresh_stats()
│       ├── COUNT total learned
│       ├── COUNT mastered (level >= 4)
│       ├── COUNT due (next_review <= NOW)
│       ├── COUNT this week / this month
│       └── Upsert into user_dictionary_stats
├── API endpoints (learned_dictionary.py)
│   ├── GET    /learned (filtered + paginated list)
│   ├── GET    /learned/stats
│   ├── GET    /learned/review-queue
│   ├── POST   /learned/review
│   ├── GET    /learned/{sign_id} (detail)
│   ├── POST   /learned/{sign_id}/star
│   ├── DELETE /learned/{sign_id}/star
│   ├── PUT    /learned/{sign_id}/note
│   ├── DELETE /learned/{sign_id}/note
│   └── GET    /learned/categories
├── Celery task: dictionary_stats_refresh.py
│   └── refresh_all_user_stats() [every hour]
│       ├── Batch process all active users
│       └── Update user_dictionary_stats table
├── Integration hooks
│   ├── exercise_service.py → add_sign_to_dictionary() on correct
│   ├── lesson_service.py → add_sign_to_dictionary() on complete
│   └── spaced_repetition_service.py → shared SM-2 logic
├── Tests
│   ├── test_auto_add_on_correct_answer
│   ├── test_no_duplicate_entries
│   ├── test_get_learned_signs_with_filters
│   ├── test_search_learned_signs
│   ├── test_review_sign_sm2
│   ├── test_mastery_level_progression
│   ├── test_review_queue_ordering
│   ├── test_toggle_star
│   ├── test_update_note
│   ├── test_get_stats
│   └── test_stats_refresh
└── Rate limiting: Dictionary 100 req/min

Week 10: Dictionary Backend Polish
├── Performance optimization
│   ├── Indexes on user_learned_signs
│   │   ├── (user_id, sign_id)
│   │   ├── (user_id, mastery_level)
│   │   ├── (user_id, next_review_at)
│   │   ├── (user_id, first_learned_at DESC)
│   │   └── (user_id, is_starred)
│   ├── Query optimization (selectinload for joins)
│   └── Redis caching for stats (TTL: 1 hour)
├── Edge cases
│   ├── Handle sign deleted from global dict
│   ├── Handle language switch
│   └── Handle bulk import of learned signs
├── Achievement integration
│   ├── "First Word" (1 learned)
│   ├── "Walking Dictionary" (100 learned)
│   ├── "Encyclopedia" (500 learned)
│   ├── "Review Master" (50 reviews in one day)
│   └── "Note Taker" (10 personal notes)
└── Admin endpoints for dictionary management


PHASE 5: SPEED SIGN GAME (Weeks 11-13)
──────────────────────────────────────────────────────────────────

Week 11: Speed Sign Backend Core
├── Models
│   ├── SpeedSignLevel
│   │   ├── level_number, language_id (unique together)
│   │   ├── time_limit_ms, signs_pool_size, options_count
│   │   ├── difficulty, sign_types[]
│   │   ├── min_score_to_pass, xp_reward, gem_reward
│   │   └── rounds_per_level
│   ├── SpeedSignSession
│   │   ├── user_id, language_id, game_mode
│   │   ├── start_level, end_level
│   │   ├── total_score, total_rounds, correct_rounds
│   │   ├── max_combo, avg_response_ms
│   │   ├── best_level_reached, status
│   │   ├── xp_earned, gems_earned
│   │   └── match_id (optional multiplayer link)
│   ├── SpeedSignRound
│   │   ├── session_id, sign_id
│   │   ├── round_number, level_number
│   │   ├── time_limit_ms, response_time_ms
│   │   ├── is_correct, user_answer, correct_answer
│   │   ├── base_points, speed_bonus
│   │   ├── combo_multiplier, round_score
│   │   └── created_at
│   ├── SpeedSignLeaderboard
│   │   ├── user_id, language_id (unique together)
│   │   ├── best_score, best_level_reached, best_combo
│   │   ├── best_avg_response_ms
│   │   ├── daily_best_score, daily_best_date
│   │   ├── total_games_played, total_rounds_played
│   │   └── overall_accuracy
│   └── SpeedSignDailyChallenge
│       ├── date (unique), language_id
│       ├── sign_ids[] (UUID array, 20 signs)
│       ├── time_limit_ms, difficulty
│       └── xp_reward, gem_reward
├── Schemas
│   ├── SpeedSignLevelResponse
│   ├── SpeedSignStartRequest / Response
│   ├── SpeedSignAnswerRequest / Response
│   ├── SpeedSignCameraAnswerRequest
│   ├── SpeedSignLevelCompleteRequest / Response
│   ├── SpeedSignCompleteRequest / Response
│   ├── SpeedSignSessionResponse
│   ├── SpeedSignStatsResponse
│   ├── SpeedSignLeaderboardResponse
│   └── DailyChallengeResponse
├── Migration: 012_create_speed_sign_tables.py
├── Service: speed_sign_service.py
│   ├── start_session()
│   │   ├── Validate level exists for language
│   │   ├── Create SpeedSignSession record
│   │   ├── Generate rounds for first level
│   │   └── Return session_id + rounds + time_limit
│   ├── generate_rounds()
│   │   ├── Fetch sign pool based on level difficulty
│   │   ├── Select 10 random signs from pool
│   │   ├── For each sign:
│   │   │   ├── Pick 3 distractors from pool
│   │   │   ├── Shuffle options
│   │   │   └── Create SpeedSignRound record
│   │   └── Return round data with options
│   ├── submit_answer()
│   │   ├── Validate round belongs to session
│   │   ├── Check answer against correct_answer
│   │   ├── Calculate speed_multiplier
│   │   │   ├── <25% time: 3.0x "LIGHTNING"
│   │   │   ├── <50% time: 2.0x "BLAZING"
│   │   │   ├── <75% time: 1.5x "FAST"
│   │   │   └── <100% time: 1.0x "OK"
│   │   ├── Calculate combo_multiplier
│   │   │   ├── 10+: 3.0x
│   │   │   ├── 8-9: 2.5x
│   │   │   ├── 5-7: 2.0x
│   │   │   ├── 3-4: 1.5x
│   │   │   └── 1-2: 1.0x
│   │   ├── Calculate round_score = base × speed × combo
│   │   ├── Update round record
│   │   ├── Auto-add sign to learned dictionary if correct
│   │   └── Return score breakdown
│   ├── submit_camera_answer()
│   │   ├── Receive landmarks + confidence + response_time
│   │   ├── Verify with ML service (threshold 0.65)
│   │   ├── Apply same scoring formula (base 15 pts for camera)
│   │   └── Return score breakdown
│   ├── complete_level()
│   │   ├── Calculate level score
│   │   ├── Check if score >= min_score_to_pass
│   │   ├── If passed: generate next level rounds
│   │   ├── If failed: trigger session completion
│   │   ├── Award level XP
│   │   └── Return pass/fail + next level data
│   ├── complete_session()
│   │   ├── Update session record with final stats
│   │   ├── Calculate total XP (correct_rounds × 3 + best_level × 5)
│   │   ├── Calculate gems (5 if level 5+, 10 if level 10+)
│   │   ├── Award XP via xp_service
│   │   ├── Update SpeedSignLeaderboard
│   │   │   ├── Check personal best
│   │   │   ├── Update best_score, best_level, best_combo
│   │   │   └── Increment total_games_played
│   │   ├── Check Speed Sign achievements
│   │   └── Return final results
│   ├── get_history()
│   │   └── Paginated session history for user
│   ├── get_stats()
│   │   └── Best scores, accuracy, games played
│   ├── get_leaderboard()
│   │   ├── Filter by language + timeframe
│   │   ├── Daily: sort by daily_best_score
│   │   ├── Weekly: sort by best_score this week
│   │   ├── All-time: sort by best_score
│   │   └── Return top 100 + user's rank
│   ├── get_daily_challenge()
│   │   ├── Fetch today's challenge
│   │   ├── Check if user already completed
│   │   └── Return challenge data + user's best today
│   └── start_daily_challenge()
│       ├── Create session linked to daily challenge
│       ├── Generate rounds from fixed sign set
│       └── Return session + rounds
├── API endpoints (speed_sign.py)
│   ├── GET  /levels
│   ├── POST /start
│   ├── POST /rounds/{round_id}/answer
│   ├── POST /rounds/{round_id}/camera-answer
│   ├── POST /level-complete
│   ├── POST /complete
│   ├── GET  /history
│   ├── GET  /stats
│   ├── GET  /leaderboard
│   ├── GET  /daily-challenge
│   └── POST /daily-challenge/start
├── WebSocket handler: speed_sign_ws.py
│   ├── handle_speed_ready()
│   ├── handle_speed_answer()
│   │   ├── Broadcast opponent_answered (no answer revealed)
│   │   └── Broadcast round_result
│   ├── handle_speed_level_up()
│   └── handle_speed_match_end()
├── Celery task: daily_challenge_generator.py
│   └── generate_daily_challenge() [daily at midnight]
│       ├── Select 20 signs based on day of week difficulty
│       ├── Create SpeedSignDailyChallenge record
│       └── Reset daily scores in leaderboard
├── Seed script: seed_speed_sign_levels.py
│   └── Create 11+ levels per language with difficulty curve
├── Data file: speed_sign_levels.json
├── Constants
│   ├── SPEED_SIGN_MULTIPLIERS
│   └── SPEED_SIGN_COMBO_THRESHOLDS
├── Integration hooks
│   ├── learned_dictionary_service → auto-add on correct answer
│   ├── achievement_service → check Speed Sign achievements
│   ├── xp_service → award Speed Sign XP
│   └── multiplayer_service → link session to match
├── Tests
│   ├── test_start_session
│   ├── test_round_generation
│   ├── test_submit_correct_answer
│   ├── test_submit_wrong_answer
│   ├── test_speed_multiplier_calculation
│   ├── test_combo_multiplier_calculation
│   ├── test_combo_reset_on_wrong
│   ├── test_level_progression
│   ├── test_level_fail
│   ├── test_complete_session
│   ├── test_leaderboard_update
│   ├── test_personal_best
│   ├── test_daily_challenge
│   ├── test_camera_answer
│   └── test_xp_gem_rewards
└── Rate limiting: Speed Sign 60 req/min

Week 12-13: Speed Sign Backend Polish
├── Performance optimization
│   ├── Indexes on speed_sign tables
│   │   ├── (user_id, status) on sessions
│   │   ├── (user_id, total_score DESC) on sessions
│   │   ├── (session_id) on rounds
│   │   ├── (language_id, best_score DESC) on leaderboard
│   │   └── (date, language_id) on daily challenges
│   ├── Redis caching for leaderboard (TTL: 5 min)
│   ├── Redis caching for daily challenge (TTL: 1 hour)
│   └── Bulk insert for round records
├── Anti-cheat measures
│   ├── Validate response_time_ms > 200ms (human minimum)
│   ├── Validate response_time_ms <= time_limit_ms
│   ├── Rate limit answer submissions
│   └── Server-side score verification
├── Edge cases
│   ├── Handle session timeout (abandon after 10 min inactive)
│   ├── Handle disconnection during multiplayer
│   ├── Handle insufficient signs in pool
│   └── Handle concurrent daily challenge starts
├── Achievement integration
│   ├── "Quick Hands" (first game)
│   ├── "Lightning Round" (answer < 1s)
│   ├── "Combo King" (10+ combo)
│   ├── "Speed Demon" (reach level 10)
│   ├── "Unstoppable" (1000+ single game)
│   ├── "Daily Grinder" (7 daily challenges)
│   ├── "Speed Champion" (top 1 daily)
│   └── "Camera Speed" (10 camera answers in Speed)
├── Leaderboard in main leaderboard API
│   └── GET /leaderboard/speed-sign
├── Multiplayer integration
│   ├── Add "speed_sign" to multiplayer game_type
│   ├── Matchmaking for Speed Sign races
│   └── WebSocket real-time scoring
└── Admin endpoints
    ├── POST /admin/speed-sign/levels
    ├── PUT  /admin/speed-sign/levels/{level_id}
    └── POST /admin/speed-sign/daily-challenge


PHASE 6: SOCIAL & MULTIPLAYER (Weeks 14-15)
──────────────────────────────────────────────────────────────────

Week 14: Social Features
├── Models
│   ├── UserFriend
│   ├── League
│   ├── LeagueSeason
│   └── LeagueParticipant
├── Schemas (social.py, multiplayer.py)
├── Migration: 006_create_social_tables.py
├── Services
│   ├── social/friends logic in user_service
│   │   ├── send_friend_request()
│   │   ├── accept_friend_request()
│   │   ├── decline_friend_request()
│   │   ├── remove_friend()
│   │   ├── search_users()
│   │   └── get_friends_list()
│   └── leaderboard_service.py
│       ├── get_weekly_rankings()
│       ├── get_all_time_rankings()
│       ├── get_friends_rankings()
│       ├── get_league_standings()
│       └── process_promotions_demotions()
├── API endpoints
│   ├── social.py (friends CRUD + search)
│   └── leaderboard.py (rankings + leagues)
├── Celery task: league_update.py
│   └── process_weekly_leagues() [every Monday]
│       ├── Calculate final rankings
│       ├── Promote top 10
│       ├── Demote bottom 5
│       ├── Create new season
│       └── Award gem rewards
├── WebSocket: notification_ws.py
│   ├── send_heart_received()
│   ├── send_friend_request()
│   ├── send_achievement_unlocked()
│   └── send_league_update()
└── Tests

Week 15: Multiplayer & Community
├── Models
│   ├── MultiplayerMatch
│   ├── MatchPlayer
│   ├── CommunityPost
│   ├── PostComment
│   └── PostLike
├── Migration: 007_create_multiplayer_tables.py, 009_create_community_tables.py
├── Services
│   ├── multiplayer_service.py
│   │   ├── create_match()
│   │   ├── join_match()
│   │   ├── start_match()
│   │   ├── submit_round_answer()
│   │   └── complete_match()
│   └── community_service.py
│       ├── create_post()
│       ├── get_feed()
│       ├── toggle_like()
│       ├── add_comment()
│       └── delete_post()
├── WebSocket: multiplayer_ws.py
│   ├── handle_player_join()
│   ├── handle_answer_submit()
│   ├── handle_round_result()
│   └── handle_match_end()
├── API endpoints
│   ├── multiplayer.py
│   └── community.py
├── Notification service (notification_service.py)
│   ├── send() (in-app + WebSocket + optional email)
│   ├── get_notifications()
│   ├── mark_read()
│   └── mark_all_read()
├── Migration: 010_create_notifications.py
├── API endpoints: notifications.py
├── Celery task: notification_tasks.py
│   ├── send_streak_reminders() [daily 8pm]
│   └── send_review_reminders() [daily 9am]
└── Tests


PHASE 7: STORY MODE & ADMIN (Weeks 16-17)
──────────────────────────────────────────────────────────────────

Week 16: Story Mode
├── Models
│   ├── Story
│   ├── StoryScene
│   └── UserStoryProgress
├── Migration: 008_create_story_tables.py
├── Service: story_service.py
│   ├── get_stories()
│   ├── start_story()
│   ├── process_scene()
│   │   ├── Handle dialogue, narration, choice, sign_practice, quiz
│   │   ├── Auto-add signs to dictionary on practice
│   │   └── Calculate scene XP
│   └── complete_story()
├── API endpoints: stories.py
├── Seed script: seed_stories.py
├── Data file: stories.json
└── Tests

Week 17: Admin & Dictionary Polish
├── Admin API endpoints (admin.py)
│   ├── CRUD for signs, units, lessons, exercises
│   ├── CRUD for achievements
│   ├── CRUD for Speed Sign levels
│   ├── Daily challenge management
│   └── Admin stats dashboard
├── Admin permissions (core/permissions.py)
├── Dictionary bookmark API (signs.py)
│   ├── POST /{sign_id}/bookmark
│   ├── DELETE /{sign_id}/bookmark
│   └── GET /bookmarks
├── Global search enhancement
│   ├── PostgreSQL full-text search (tsvector)
│   └── Meilisearch integration (optional)
├── Email utility (utils/email.py)
│   ├── send_welcome_email()
│   ├── send_password_reset()
│   └── send_notification_email()
└── Tests


PHASE 8: POLISH & LAUNCH (Weeks 18-19)
──────────────────────────────────────────────────────────────────

Week 18: Testing & Optimization
├── Unit tests (pytest)
│   ├── All services > 80% coverage
│   ├── All API endpoints
│   └── Edge cases + error handling
├── Integration tests
│   ├── Full lesson flow (start → exercises → complete)
│   ├── Full Speed Sign flow (start → rounds → levels → complete)
│   ├── Full dictionary flow (learn → review → master)
│   └── Full auth flow (register → login → refresh → logout)
├── Load testing
│   ├── Speed