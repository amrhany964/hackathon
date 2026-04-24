# hackathon
https://jupyterlab-01kpzjpnbzqd6p55drj0wgx1d1.studio.lightning.ai/lab/tree/domain_xlm_roberta_reviews
https://d1y8zswxjnvm73.cloudfront.net/team-leader
https://chatgpt.com/c/69eb149e-8ffc-83ea-8759-73c229e6a8ce


https://chatgpt.com/share/69eb335f-4098-83ea-88a0-41b3993d3456
def build_submission(test_raw, test_aspect_df):
    pred_map = {}

    for _, row in test_aspect_df.iterrows():
        review_id = int(row["review_id"])
        aspect = row["aspect"]
        sentiment = row["predicted_sentiment"]

        if review_id not in pred_map:
            pred_map[review_id] = {}

        pred_map[review_id][aspect] = sentiment

    submission = []

    for _, row in test_raw.iterrows():
        review_id = int(row["review_id"])

        aspects = parse_obj(row["aspects"])

        if aspects is None:
            aspects = []

        if isinstance(aspects, str):
            aspects = [aspects]

        clean_aspects = []
        aspect_sentiments = {}

        for aspect in aspects:
            aspect = str(aspect).strip()

            if aspect == "none":
                continue

            if aspect not in ASPECTS:
                continue

            if review_id in pred_map and aspect in pred_map[review_id]:
                clean_aspects.append(aspect)
                aspect_sentiments[aspect] = pred_map[review_id][aspect]

        submission.append({
            "review_id": review_id,
            "aspects": clean_aspects,
            "aspect_sentiments": aspect_sentiments
        })

    return submission


submission = build_submission(test_raw, test_df)

submission[:3]
