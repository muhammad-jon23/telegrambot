if kind == "fact":
                        await create_fact(app)

                    elif kind == "nature":
                        await create_video(
                            app,
                            "peaceful nature landscape vertical",
                            "🌿 Osoyishta tabiat videosi"
                        )

                    elif kind == "motivation":
                        await create_video(
                            app,
                            "cinematic motivation inspirational vertical",
                            "🔥 Motivatsion video"
                        )

                    elif kind == "peaceful":
                        await create_video(
                            app,
                            "peaceful sunset meditation nature vertical",
                            "✨ Osoyishta va hayotiy nasihat"
                        )

        except Exception as e:
            print("Jadval xatosi:", e)

        await asyncio.sleep(25)


# =====================================
# 12. BOTni ISHGA TUSHIRISH
# =====================================

async def post_init(app):
    # Bot qayta ishga tushganda saqlangan navbatni
    # egasiga yana ko'rsatish
    for item in list(pending.values()):
        if item["type"] == "video":
            media = item.get("media", "")

            if os.path.isfile(media):
                await send_preview(app, item)

        else:
            await send_preview(app, item)

    app.create_task(scheduled_content(app))


def main():
    print("Hammasi Yaxshi bot ishga tushmoqda...")

    app = (
        Application.builder()
        .token(TOKEN)
        .post_init(post_init)
        .build()
    )

    app.add_handler(CommandHandler("start", start))
    app.add_handler(CommandHandler("queue", queue_command))
    app.add_handler(CommandHandler("cancelall", cancel_all))
    app.add_handler(CommandHandler("fact", fact_command))
    app.add_handler(CommandHandler("video", video_command))

    app.add_handler(
        CallbackQueryHandler(callback_handler)
    )

    app.add_handler(
        MessageHandler(
            filters.TEXT & ~filters.COMMAND,
            owner_message
        )
    )

    app.add_handler(
        MessageHandler(
            filters.PHOTO | filters.VIDEO,
            owner_message
        )
    )

    app.run_polling(
        allowed_updates=["message", "callback_query"]
    )


if name == "main":
    main()
