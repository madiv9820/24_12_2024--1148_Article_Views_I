# [1148. Article Views I](https://leetcode.com/problems/article-views-i?envType=study-plan-v2&envId=top-sql-50)

- ### Intuition
    The task is to identify authors who have viewed at least one of their own articles. The data structure includes four columns:

    - `article_id`: The ID of the article being viewed.
    - `author_id`: The ID of the author who wrote the article.
    - `viewer_id`: The ID of the viewer who viewed the article.
    - `view_date`: The date the article was viewed.

    The main observation here is that when `author_id` equals `viewer_id` for a particular `article_id`, it indicates that the author has viewed their own article. Our goal is to find all such authors and return a distinct list of `author_id`s, sorted in ascending order.

- ### Approach
    1. **Filter Rows Where Author Viewed Their Own Article**:
        - We need to identify the rows where `author_id` is the same as `viewer_id`. This indicates that the author has viewed one of their own articles.

    2. **Select Distinct Author IDs**:
        - Since an author might view multiple articles on different dates, we should only include each author once. Therefore, we need to select distinct `author_id` values.

    3. **Sort the Results**:
        - Once we have the distinct list of authors, we need to sort the results in ascending order of `author_id` to meet the requirements.

    4. **Return the Results**:
        - The final result should be a table with a single column `id`, which will contain the distinct `author_id` values of authors who have viewed their own articles.

- ### Code Implementation
    - **SQL:**
        ```sql []
        SELECT DISTINCT(author_id) as id FROM Views
        WHERE author_id = viewer_id
        ORDER BY id
        ```
    - **Pyspark:**
        ```python3 []
        def article_views(views: pyspark.sql.dataframe.DataFrame) -> pyspark.sql.dataframe.DataFrame:
            output = views.filter(views.author_id == views.viewer_id)\
                            .select(views.author_id.alias('id'))\
                            .distinct()\
                            .orderBy("author_id")
            return output
        ```
    - **Pandas**
        ```python3 []
        def article_views(views: pd.DataFrame) -> pd.DataFrame:
            output = views[(views.author_id == views.viewer_id)].author_id.sort_values().unique()
            output = pd.DataFrame(output, columns = ['id'])
            return output
        ```