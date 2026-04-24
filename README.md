# hackathon
https://jupyterlab-01kpzjpnbzqd6p55drj0wgx1d1.studio.lightning.ai/lab/tree/domain_xlm_roberta_reviews
https://d1y8zswxjnvm73.cloudfront.net/team-leader
https://chatgpt.com/c/69eb149e-8ffc-83ea-8759-73c229e6a8ce


https://chatgpt.com/share/69eb335f-4098-83ea-88a0-41b3993d3456
test_raw = pd.read_excel(TEST_PATH)

print("Test shape:", test_raw.shape)
print(test_raw.columns)
test_raw.head()


def explode_test(df):
    rows = []

    for _, row in df.iterrows():
        review_id = row["review_id"]
        review_text = clean_text(row["review_text"])

        aspects = parse_obj(row["aspects"])

        if aspects is None:
            aspects = []

        if isinstance(aspects, str):
            aspects = [aspects]

        for aspect in aspects:
            aspect = str(aspect).strip()

            if aspect == "none":
                continue

            if aspect not in ASPECTS:
                continue

            rows.append({
                "review_id": review_id,
                "review_text": review_text,
                "star_rating": row.get("star_rating", ""),
                "platform": row.get("platform", ""),
                "business_category": row.get("business_category", ""),
                "aspect": aspect
            })

    return pd.DataFrame(rows)


test_df = explode_test(test_raw)
test_df["input_text"] = test_df.apply(build_input, axis=1)

print(test_df.shape)
test_df.head()
