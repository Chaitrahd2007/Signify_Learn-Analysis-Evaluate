# 🗺️ Backend Development Roadmap

> A phase-by-phase engineering plan spanning **19 weeks** covering foundation, gamification, ML/Camera, dictionary, speed-sign game, social features, admin tools & launch prep.

---

## 🏗️ PHASE 1: FOUNDATION
### 📆 Weeks 1 – 3

---

### 📅 Week 1: Project Setup & Auth

#### 🔧 Project Setup
- Initialize FastAPI project structure
- Configure `pyproject.toml` + `requirements.txt`
- Set up Docker + `docker-compose.yml`
  - PostgreSQL 15 container
  - Redis 7 container
  - Backend container
- `config.py` — Pydantic Settings from `.env`
- `database.py` — Async SQLAlchemy engine + session
- Alembic setup (`alembic.ini` + `env.py`)

#### 🔐 Core Security Module
- JWT token creation (access + refresh)
- Password hashing (`bcrypt`)
- Token decoding + validation
- Token expiration handling

#### 🗂 Models & Schemas
- `User`, `UserSession` (SQLAlchemy)
- Auth Schemas (Pydantic v2)

#### ⚙️ Auth Service
| Function | Responsibilities |
|----------|------------------|
| `register_user()` | Validate email/username → Hash password → Create user → Generate JWTs → Send welcome email (Celery) |
| `authenticate_user()` | Verify credentials → Check `is_active` → Generate tokens → Create session |
| `refresh_tokens()` | Validate refresh token → Check expiration → Rotate tokens → Update session |
| `verify_google_token()` | Verify Google ID token → Find/create user → Generate JWTs |
| `logout()` | Invalidate session |

#### 🌐 API & Dependencies
- `api/v1/auth.py` — Auth endpoints
- `api/deps.py`
  - `get_db()` → `AsyncSession`
  - `get_current_user()` → JWT decode
  - `get_current_active_user()`

#### 🛡 Core Config
- `core/exceptions.py` — Custom exceptions
- CORS middleware
- Rate limiting (SlowAPI) → Auth endpoints: **5 req/min**
- Health check endpoint

#### 💾 Database & Docs
- Migration: `001_create_users.py`
- `.env.example`

#### 🧪 Tests
- `conftest.py` (`test_db`, `test_client`, `test_user`)
- `test_register`, `test_login`, `test_refresh`, `test_logout`, `test_google_oauth`

---

### 📅 Week 2: Content System

#### 🗂 Models
- `SignLanguage`, `Unit`, `Lesson`
- `Sign` (Media URLs, ML labels, Landmark data)
- `LessonSign` (Join table)

#### 📦 Schemas
- `UnitResponse` (with progress)
- `LessonResponse` (with signs + exercises)
- `SignResponse` (with media + mastery)
- `SignSearchQuery`

#### 💾 Database
- Migration: `002_create_content_tables.py`

#### ⚙️ Services
**`lesson_service.py`**
- `get_units_with_progress()` — Fetch units → Join `user_unit_progress` → Completion %
- `get_lesson_with_exercises()` — Lesson + signs + ordered exercises
- `get_sign_detail()`

**`search_service.py`**
- `search_signs()` — Category / Difficulty / Letter filters
- `search_signs_fulltext()` — PostgreSQL `tsvector`
- `get_categories()`

#### 🌐 API Endpoints
| File | Endpoints |
|------|-----------|
| `units.py` | `GET /`, `GET /{id}`, `GET /{id}/lessons` |
| `lessons.py` | `GET /{id}`, `GET /{id}/exercises` |
| `signs.py` | `GET /`, `GET /{id}`, `GET /categories`, `GET /search` |

#### 🌱 Seed & Utils
- `seed_data.py` (languages, units, lessons)
- `seed_signs.py` (from JSON + S3 URLs)
- Data files: `signs_asl.json`, `signs_isl.json`, `signs_bsl.json`
- `utils/file_upload.py` — `upload_to_s3()`, `delete_from_s3()`, `get_presigned_url()`

#### 🧪 Tests & Limits
- `test_get_units`, `test_get_lessons`, `test_search_signs`
- Rate limit: **100 req/min**

---

### 📅 Week 3: Exercise System

#### 🗂 Models
- `Exercise` (9 types, JSONB options/answers)
- `UserExerciseAttempt`
- `UserLessonProgress`

#### 📦 Schemas
- `ExerciseResponse`, `ExerciseSubmitRequest`, `ExerciseResult`, `CameraVerifyRequest`

#### 💾 Database
- Migrations: `003_create_exercises.py`, `004_create_progress_tables.py`

#### ⚙️ Services
**`exercise_service.py`**
- `submit_answer()` — Validate → Record attempt → Calculate XP → Heart loss / Dictionary auto-add → Return result
- `verify_camera_answer()` — Landmarks + confidence → ML check → Threshold → Record → Feedback
- `check_answer_correctness()`

**`lesson_service.py`** (continued)
- `start_lesson_session()` — Hearts check → Redis session ID → Ordered exercises
- `complete_lesson()` — Score + stars → Award XP → Update progress → Achievements → Dictionary auto-add

#### 🌐 API Endpoints
| File | Endpoints |
|------|-----------|
| `exercises.py` | `POST /{id}/submit`, `POST /camera/verify` |
| `lessons.py` | `POST /{id}/start`, `POST /{id}/complete` |

#### 🧪 Tests & Seed
- `test_submit_correct_answer`, `test_submit_wrong_answer`
- `test_camera_verify`, `test_start_lesson`, `test_complete_lesson`
- Seed exercises in `seed_data.py`

---

## 🎮 PHASE 2: GAMIFICATION
### 📆 Weeks 4 – 5

---

### 📅 Week 4: XP, Hearts & Streaks

#### 🗂 Models & Schemas
- Models: `DailyXpLog`, `HeartTransaction`, `UserSignMastery`
- Schemas: `HeartsResponse`, `StreakResponse`, `LevelProgress`
- Migration: `005_create_gamification_tables.py`

#### ⚙️ Services

**`xp_service.py`**
- `award_xp()` — Compute XP → Update totals & daily log → Level change → Streak trigger → Return breakdown + `level_up`
- `calculate_level()` → `floor(sqrt(xp/50)) + 1`
- `xp_for_level()`, `xp_progress_in_level()`

**`heart_service.py`**
- `lose_heart()` — Premium bypass → Decrement → Log → `can_continue`
- `refill_hearts()` — Time check → +1 up to max → Log
- `refill_with_gems()` — 450 gems → Deduct → Max hearts
- `share_heart()` — Friendship → Daily limit → Transfer → Notify
- `get_heart_status()`

**`streak_service.py`**
- `update_streak()`, `check_streak_reset()`
- `activate_freeze()` — 100 gems → Freeze today
- `get_streak_calendar()` — 30-day history

**`spaced_repetition_service.py`**
- `calculate_next_review()` — **SM-2** (quality 0-5, ease, interval)
- `process_review_quality()` — Map results → quality

#### ⏰ Celery Tasks
| Task | Schedule | Action |
|------|----------|--------|
| `heart_refill.py` | Every 30 min | Refill eligible users (bulk log) |
| `streak_check.py` | Daily midnight | Reset streaks (no activity + no freeze) |

#### 🌐 API Endpoints (`gamification.py`)
```
GET  /hearts
POST /hearts/refill
POST /hearts/share
GET  /streak
POST /streak/freeze
```

#### 📐 Constants (`core/constants.py`)
- `XP_REWARDS`, `HEART_CONFIG`, `STREAK_MILESTONES`

#### 🧪 Tests & Limits
- `test_xp_award`, `test_level_calculation`
- `test_heart_loss`, `test_heart_refill_time`, `test_heart_refill_gems`, `test_heart_share`
- `test_streak_increment`, `test_streak_reset`, `test_streak_freeze`
- `test_spaced_repetition`
- Rate limit: **100 req/min**

---

### 📅 Week 5: Gems, Achievements & Progress

#### 🗂 Models & Schemas
- Models: `Achievement`, `UserAchievement`, `GemTransaction`
- Schemas: `GemsResponse`, `AchievementResponse`, `ProgressOverview`, `WeeklyProgress`

#### ⚙️ Services

**`achievement_service.py`**
- `check_achievements()` — Called post-XP → Compare requirement → Unlock → Award XP/gems → Notify + WS event
- `unlock_achievement()`
- `get_all_achievements()` (with earned status)
- `get_recent_achievements()`

**`progress_service.py`**
- `get_overview()` — Completion %, units/lessons/signs, XP+level, dict size, Speed Sign best
- `get_weekly_progress()` — 7-day XP, lessons, minutes, goals, speed games
- `get_milestones()`, `update_unit_progress()`, `update_lesson_progress()`, `update_sign_mastery()`

**Gem economy** — Integrated with `xp_service`

#### 🌐 API Endpoints
| File | Endpoints |
|------|-----------|
| `gamification.py` | `GET /gems`, `POST /gems/spend` |
| `achievements.py` | `GET /`, `GET /recent` |
| `progress.py` | `GET /overview`, `GET /weekly`, `GET /milestones` |

#### 🌱 Seed & Constants
- `seed_achievements.py` + `achievements.json`
- `ACHIEVEMENT_DEFINITIONS`, `GEM_CONFIG`

#### 🧪 Tests
- `test_gem_earn`, `test_gem_spend`
- `test_achievement_unlock`, `test_achievement_check`
- `test_progress_overview`, `test_weekly_progress`

---

## 🤖 PHASE 3: ML & CAMERA
### 📆 Weeks 6 – 8

---

### 📅 Week 6: ML Infrastructure

#### 🧠 ML Service (`services/ml_service.py`)
- `load_model()` — ONNX from disk → LRU cache → Per-language support
- `predict_static()` — 225-feature vector → ONNX → Softmax → Top-5 + confidence
- `predict_dynamic()` — `30×225` matrix → GRU ONNX → Prediction + confidence
- `process_landmarks()` — Normalize → Center on wrist → Scale

#### 📍 Landmark Processor (`ml/landmark_processor.py`)
- `normalize_landmarks()`, `center_landmarks()`, `validate_landmarks()`

#### 📦 Model Files
- `asl_static.onnx`, `asl_dynamic.onnx`
- `isl_static.onnx`, `isl_dynamic.onnx`

#### 🌐 API Endpoints (`camera.py`)
```
POST /predict           → Static sign
POST /predict-sequence  → Dynamic sign
```

#### ⚙️ Config & Limits
| Setting | Value |
|---------|-------|
| `ML_MODEL_PATH` | *(configurable)* |
| `ML_CONFIDENCE_THRESHOLD` | `0.7` |
| `ML_SPEED_CONFIDENCE_THRESHOLD` | `0.65` |
| Rate limit | **30 req/min** |

#### 🧪 Tests
- `test_predict_static`, `test_predict_dynamic`, `test_landmark_processing`

---

### 📅 Weeks 7 – 8: Camera Integration & Refinement
- **Camera Verify Enhancement:** Server validation + confidence fallback if client low
- **`exercise_service` Integration:** Camera exercise type, +10 XP, heart loss on wrong
- **`progress_service` Integration:** Sign mastery updates + confidence logging
- **Performance:** Model caching (load once → serve many), async inference, optional batching
- **Tests:** `test_camera_exercise_flow`, `test_server_fallback_prediction`

---

## 📖 PHASE 4: LEARNED DICTIONARY
### 📆 Weeks 9 – 10

---

### 📅 Week 9: Dictionary Backend Core

#### 🗂 Models

**`UserLearnedSign`**
- Unique `(user_id, sign_id)`
- `first_learned_at`, `first_lesson_id`, `learned_via`
- `mastery_level` (1-5)
- SM-2: `times_reviewed`, `last_reviewed_at`, `next_review_at`, `ease_factor`, `interval_days`
- `correct_count`, `incorrect_count`, `total_time_spent`
- `is_starred`, `is_hidden`, `personal_note`

**`UserDictionaryStats`**
- Unique `(user_id, language_id)`
- `total_learned`, `mastered_count`, `due_for_review`
- `learned_this_week`, `learned_this_month`
- `category_counts` (JSONB)

#### 📦 Schemas
- `LearnedSignResponse`
- `LearnedSignQuery` (filters + sort + pagination)
- `DictionaryStatsResponse`, `ReviewQueueResponse`
- `ReviewSubmitRequest` (sign_id, quality 0-5, time_spent)
- `PersonalNoteUpdate`, `LearnedSignDetailResponse`

#### 💾 Database
- Migration: `011_create_learned_dictionary.py`

#### ⚙️ Service (`learned_dictionary_service.py`)

| Function | Description |
|----------|-------------|
| `add_sign_to_dictionary()` | Upsert on correct answer → Initial SM-2 (+1 day) → Stats refresh |
| `get_learned_signs()` | Filter (lang/cat/mastery/starred/due) + sort + ILIKE search + pagination |
| `get_learned_sign_detail()` | Full sign + metadata + attempt history |
| `get_stats()` | Cached stats + mastery distribution |
| `get_review_queue()` | `next_review_at <= NOW()` ordered ASC |
| `review_sign()` | SM-2 update + mastery ±1 + XP +3 |
| `toggle_star()`, `update_note()` | Simple state updates |
| `get_categories()` | Group learned signs by category |
| `refresh_stats()` | Recount totals + upsert `user_dictionary_stats` |

**🔗 `add_sign_to_dictionary()` called from:**
- `exercise_service.submit_answer()` (on correct)
- `exercise_service.verify_camera_answer()` (on correct)
- `speed_sign_service.submit_answer()` (on correct)
- `story_service.process_scene()` (on sign practice)
- `signs` API (manual add from global dict)

#### 🌐 API Endpoints (`learned_dictionary.py`)
```
GET    /learned              → Filtered + paginated list
GET    /learned/stats
GET    /learned/review-queue
POST   /learned/review
GET    /learned/{sign_id}    → Detail
POST   /learned/{sign_id}/star
DELETE /learned/{sign_id}/star
PUT    /learned/{sign_id}/note
DELETE /learned/{sign_id}/note
GET    /learned/categories
```

#### ⏰ Celery Task
- `dictionary_stats_refresh.py` — `refresh_all_user_stats()` **every hour** → Batch active users

#### 🔗 Integration Hooks
- `exercise_service.py` → `add_sign_to_dictionary()` on correct
- `lesson_service.py` → `add_sign_to_dictionary()` on complete
- `spaced_repetition_service.py` → Shared SM-2 logic

#### 🧪 Tests & Limits
- `test_auto_add_on_correct_answer`, `test_no_duplicate_entries`
- `test_get_learned_signs_with_filters`, `test_search_learned_signs`
- `test_review_sign_sm2`, `test_mastery_level_progression`, `test_review_queue_ordering`
- `test_toggle_star`, `test_update_note`, `test_get_stats`, `test_stats_refresh`
- Rate limit: **100 req/min**

---

### 📅 Week 10: Dictionary Backend Polish

#### ⚡ Performance
- **Indexes on `user_learned_signs`:**
  - `(user_id, sign_id)`, `(user_id, mastery_level)`
  - `(user_id, next_review_at)`, `(user_id, first_learned_at DESC)`
  - `(user_id, is_starred)`
- Query optimization → `selectinload` for joins
- Redis caching for stats — TTL **1 hour**

#### 🧩 Edge Cases
- Sign deleted from global dict
- Language switch
- Bulk import of learned signs

#### 🏆 Achievement Integration
| Achievement | Requirement |
|-------------|-------------|
| First Word | 1 learned |
| Walking Dictionary | 100 learned |
| Encyclopedia | 500 learned |
| Review Master | 50 reviews in one day |
| Note Taker | 10 personal notes |

#### 🛠 Admin
- Admin endpoints for dictionary management

---

## ⚡ PHASE 5: SPEED SIGN GAME
### 📆 Weeks 11 – 13

---

### 📅 Week 11: Speed Sign Backend Core

#### 🗂 Models

| Model | Key Fields |
|-------|------------|
| `SpeedSignLevel` | `level_number + language_id` unique, `time_limit_ms`, `signs_pool_size`, `options_count`, `difficulty`, `sign_types[]`, `min_score_to_pass`, `xp_reward`, `gem_reward`, `rounds_per_level` |
| `SpeedSignSession` | user, language, `game_mode`, `start_level`, `end_level`, `total_score`, `total_rounds`, `correct_rounds`, `max_combo`, `avg_response_ms`, `best_level_reached`, `status`, `xp_earned`, `gems_earned`, optional `match_id` |
| `SpeedSignRound` | `session_id`, `sign_id`, `round_number`, `level_number`, `time_limit_ms`, `response_time_ms`, `is_correct`, `user_answer`, `correct_answer`, `base_points`, `speed_bonus`, `combo_multiplier`, `round_score`, `created_at` |
| `SpeedSignLeaderboard` | `user_id + language_id` unique, `best_score`, `best_level_reached`, `best_combo`, `best_avg_response_ms`, `daily_best_score`, `daily_best_date`, `total_games_played`, `total_rounds_played`, `overall_accuracy` |
| `SpeedSignDailyChallenge` | `date` unique, `language_id`, `sign_ids[]` (20 UUIDs), `time_limit_ms`, `difficulty`, rewards |

#### 📦 Schemas
- `SpeedSignLevelResponse`
- `SpeedSignStartRequest` / `Response`
- `SpeedSignAnswerRequest` / `Response`
- `SpeedSignCameraAnswerRequest`
- `SpeedSignLevelCompleteRequest` / `Response`
- `SpeedSignCompleteRequest` / `Response`
- `SpeedSignSessionResponse`
- `SpeedSignStatsResponse`
- `SpeedSignLeaderboardResponse`
- `DailyChallengeResponse`

#### 💾 Database
- Migration: `012_create_speed_sign_tables.py`

#### ⚙️ Service (`speed_sign_service.py`)

**Core Flow**
- `start_session()` → Validate level → Create session → Generate first-level rounds → Return session + rounds + time_limit
- `generate_rounds()` → Fetch pool by difficulty → 10 random signs → +3 distractors each → Shuffle → Create rounds
- `submit_answer()` → Validate → Check answer → Score → Auto-add to dictionary → Return breakdown
- `submit_camera_answer()` → Landmarks + confidence (threshold 0.65) → Base 15 pts → Same scoring
- `complete_level()` → Level score vs `min_score_to_pass` → Next level or finish → Award XP
- `complete_session()` → Final stats → XP/gems → Leaderboard update → Achievements → Return results

**🎯 Speed Multiplier**
| Response Time | Multiplier | Rank |
|---------------|------------|------|
| `<25%` of time | **3.0×** | LIGHTNING |
| `<50%` of time | **2.0×** | BLAZING |
| `<75%` of time | **1.5×** | FAST |
| `<100%` of time | **1.0×** | OK |

**🔥 Combo Multiplier**
| Combo | Multiplier |
|-------|------------|
| 10+ | **3.0×** |
| 8–9 | **2.5×** |
| 5–7 | **2.0×** |
| 3–4 | **1.5×** |
| 1–2 | **1.0×** |

**📊 Scoring**
```text
round_score = base × speed_multiplier × combo_multiplier
```

**💰 Session Rewards**
```text
total_xp = (correct_rounds × 3) + (best_level × 5)
```
| Milestone | Gems |
|-----------|------|
| Level 5+ | 5 |
| Level 10+ | 10 |

**📈 Other Methods**
- `get_history()` — Paginated session history
- `get_stats()` — Best scores, accuracy, games
- `get_leaderboard()` — Language + timeframe (daily/weekly/all-time) → Top 100 + user rank
- `get_daily_challenge()` — Today's challenge + user completion status
- `start_daily_challenge()` — Session linked to daily challenge + fixed rounds

#### 🌐 API Endpoints (`speed_sign.py`)
```
GET  /levels
POST /start
POST /rounds/{round_id}/answer
POST /rounds/{round_id}/camera-answer
POST /level-complete
POST /complete
GET  /history
GET  /stats
GET  /leaderboard
GET  /daily-challenge
POST /daily-challenge/start
```

#### 🔌 WebSocket (`speed_sign_ws.py`)
- `handle_speed_ready()`
- `handle_speed_answer()` → Broadcast `opponent_answered` (no answer revealed) + `round_result`
- `handle_speed_level_up()`
- `handle_speed_match_end()`

#### ⏰ Celery Task
- `daily_challenge_generator.py` — `generate_daily_challenge()` **daily at midnight**
  - Select 20 signs by day-of-week difficulty
  - Create daily challenge record
  - Reset daily leaderboard scores

#### 🌱 Seed & Constants
- `seed_speed_sign_levels.py` — 11+ levels/language with difficulty curve
- `speed_sign_levels.json`
- `SPEED_SIGN_MULTIPLIERS`, `SPEED_SIGN_COMBO_THRESHOLDS`

#### 🔗 Integration Hooks
- `learned_dictionary_service` → Auto-add on correct
- `achievement_service` → Speed Sign achievements
- `xp_service` → Award Speed Sign XP
- `multiplayer_service` → Link session to match

#### 🧪 Tests & Limits
- `test_start_session`, `test_round_generation`
- `test_submit_correct_answer`, `test_submit_wrong_answer`
- `test_speed_multiplier_calculation`, `test_combo_multiplier_calculation`, `test_combo_reset_on_wrong`
- `test_level_progression`, `test_level_fail`, `test_complete_session`
- `test_leaderboard_update`, `test_personal_best`
- `test_daily_challenge`, `test_camera_answer`, `test_xp_gem_rewards`
- Rate limit: **60 req/min**

---

### 📅 Weeks 12 – 13: Speed Sign Backend Polish

#### ⚡ Performance
- **Indexes:**
  - `(user_id, status)` on sessions
  - `(user_id, total_score DESC)` on sessions
  - `(session_id)` on rounds
  - `(language_id, best_score DESC)` on leaderboard
  - `(date, language_id)` on daily challenges
- **Redis Caching:**
  - Leaderboard → TTL **5 min**
  - Daily challenge → TTL **1 hour**
- Bulk insert for round records

#### 🛡 Anti-Cheat
- Validate `response_time_ms > 200ms` (human minimum)
- Validate `response_time_ms <= time_limit_ms`
- Rate limit answer submissions
- Server-side score verification

#### 🧩 Edge Cases
- Session timeout — abandon after 10 min inactive
- Multiplayer disconnect handling
- Insufficient signs in pool
- Concurrent daily challenge starts

#### 🏆 Achievement Integration
| Achievement | Requirement |
|-------------|-------------|
| Quick Hands | First game |
| Lightning Round | Answer < 1s |
| Combo King | 10+ combo |
| Speed Demon | Reach level 10 |
| Unstoppable | 1000+ single game |
| Daily Grinder | 7 daily challenges |
| Speed Champion | Top 1 daily |
| Camera Speed | 10 camera answers in Speed |

#### 🏅 Leaderboard Integration
```
GET /leaderboard/speed-sign
```

#### 👥 Multiplayer Integration
- Add `speed_sign` to multiplayer `game_type`
- Matchmaking for Speed Sign races
- WebSocket real-time scoring

#### 🛠 Admin Endpoints
```
POST /admin/speed-sign/levels
PUT  /admin/speed-sign/levels/{level_id}
POST /admin/speed-sign/daily-challenge
```

---

## 👥 PHASE 6: SOCIAL & MULTIPLAYER
### 📆 Weeks 14 – 15

---

### 📅 Week 14: Social Features

#### 🗂 Models
- `UserFriend`, `League`, `LeagueSeason`, `LeagueParticipant`
- Schemas: `social.py`, `multiplayer.py`
- Migration: `006_create_social_tables.py`

#### ⚙️ Services

**Friends (in `user_service`)**
- `send_friend_request()`, `accept_friend_request()`, `decline_friend_request()`
- `remove_friend()`, `search_users()`, `get_friends_list()`

**`leaderboard_service.py`**
- `get_weekly_rankings()`, `get_all_time_rankings()`
- `get_friends_rankings()`, `get_league_standings()`
- `process_promotions_demotions()`

#### 🌐 API Endpoints
- `social.py` — Friends CRUD + search
- `leaderboard.py` — Rankings + leagues

#### ⏰ Celery Task
- `league_update.py` — `process_weekly_leagues()` **every Monday**
  - Calculate final rankings
  - Promote top 10 / demote bottom 5
  - Create new season
  - Award gem rewards

#### 🔌 WebSocket (`notification_ws.py`)
- `send_heart_received()`, `send_friend_request()`
- `send_achievement_unlocked()`, `send_league_update()`

---

### 📅 Week 15: Multiplayer & Community

#### 🗂 Models
- `MultiplayerMatch`, `MatchPlayer`
- `CommunityPost`, `PostComment`, `PostLike`
- Migrations: `007_create_multiplayer_tables.py`, `009_create_community_tables.py`
- Notifications: `010_create_notifications.py`

#### ⚙️ Services

**`multiplayer_service.py`**
- `create_match()`, `join_match()`, `start_match()`
- `submit_round_answer()`, `complete_match()`

**`community_service.py`**
- `create_post()`, `get_feed()`, `toggle_like()`
- `add_comment()`, `delete_post()`

**`notification_service.py`**
- `send()` — In-app + WebSocket + optional email
- `get_notifications()`, `mark_read()`, `mark_all_read()`

#### 🔌 WebSocket (`multiplayer_ws.py`)
- `handle_player_join()`, `handle_answer_submit()`
- `handle_round_result()`, `handle_match_end()`

#### 🌐 API Endpoints
- `multiplayer.py`, `community.py`, `notifications.py`

#### ⏰ Celery Tasks (`notification_tasks.py`)
- `send_streak_reminders()` — Daily **8 PM**
- `send_review_reminders()` — Daily **9 AM**

---

## 📖 PHASE 7: STORY MODE & ADMIN
### 📆 Weeks 16 – 17

---

### 📅 Week 16: Story Mode

#### 🗂 Models
- `Story`, `StoryScene`, `UserStoryProgress`
- Migration: `008_create_story_tables.py`

#### ⚙️ Service (`story_service.py`)
- `get_stories()`, `start_story()`
- `process_scene()` — Handles Dialogue / Narration / Choice / Sign practice / Quiz
  - Auto-add signs to dictionary on practice
  - Calculate scene XP
- `complete_story()`

#### 🌐 API & Data
- API: `stories.py`
- Seed: `seed_stories.py` + `stories.json`

---

### 📅 Week 17: Admin & Dictionary Polish

#### 🛠 Admin API (`admin.py`)
- CRUD for: Signs, Units, Lessons, Exercises, Achievements
- CRUD for: Speed Sign levels + Daily challenges
- Admin stats dashboard

#### 🔐 Permissions (`core/permissions.py`)
- Role-based admin permissions

#### 🔖 Dictionary Bookmark API (`signs.py`)
```
POST   /{sign_id}/bookmark
DELETE /{sign_id}/bookmark
GET    /bookmarks
```

#### 🔍 Global Search Enhancement
- PostgreSQL full-text search (`tsvector`)
- Meilisearch integration (optional)

#### ✉️ Email Utility (`utils/email.py`)
- `send_welcome_email()`, `send_password_reset()`, `send_notification_email()`

---

## 🚀 PHASE 8: POLISH & LAUNCH
### 📆 Weeks 18 – 19

---

### 📅 Week 18: Testing & Optimization

#### 🧪 Unit Tests (pytest)
- All services — **>80% coverage** target
- All API endpoints
- Edge cases + error handling

#### 🔗 Integration Tests
| Flow | Path |
|------|------|
| Lesson | `start → exercises → complete` |
| Speed Sign | `start → rounds → levels → complete` |
| Dictionary | `learn → review → master` |
| Auth | `register → login → refresh → logout` |

#### 📊 Load Testing
- Speed Sign game load (concurrent games, WS stress)
- API endpoints under high concurrency
- Database query performance
- Redis performance under sustained load
- WebSocket connection scaling

---

### 📅 Week 19: Final Polish & Launch Prep
- End-to-end QA across all modules
- Security audit (JWT, rate limits, anti-cheat, SQL injection)
- Documentation finalization (OpenAPI / Swagger)
- Production Docker + CI/CD pipeline
- Monitoring & logging (Sentry, Prometheus, Grafana)
- Soft launch checklist & rollback plan

---

> ✅ **End of Roadmap** — Ready for structured backend development execution.
