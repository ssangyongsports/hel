---
title: ワークフローの成果物を削除する
intro: '{% data variables.product.product_name %} で期限切れになる前に成果物を削除することで、使用済みの {% data variables.product.prodname_actions %} ストレージを再利用できます。'
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
shortTitle: Remove workflow artifacts
---

{% data reusables.actions.enterprise-beta %}
{% data reusables.actions.enterprise-github-hosted-runners %}
{% data reusables.actions.ae-beta %}

## 成果物を削除する

{% warning %}

**警告:** いったん削除された成果物をリストアすることはできません。

{% endwarning %}

{% data reusables.repositories.permissions-statement-write %}

{% data reusables.github-actions.artifact-log-retention-statement %}

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.actions-tab %}
{% data reusables.repositories.navigate-to-workflow %}
{% data reusables.repositories.view-run %}
1. **Artifacts（成果物）**の下で、
削除したい成果物の隣の{% octicon "trash" aria-label="The trash icon" %}をクリックしてください。
    {% ifversion fpt or ghes > 3.0 or ghae or ghec %}
    ![成果物の削除のドロップダウンメニュー](/assets/images/help/repository/actions-delete-artifact-updated.png)
    {% else %}
    ![成果物の削除のドロップダウンメニュー](/assets/images/help/repository/actions-delete-artifact.png)
    {% endif %}

## 成果物の保持期間を設定する

成果物とログの保持期間は、リポジトリ、Organization、および Enterprise レベルで設定できます。 詳しい情報については、「[使用制限、支払い、および管理](/actions/reference/usage-limits-billing-and-administration#artifact-and-log-retention-policy)」を参照してください。

ワークフローの `actions/upload-artifact` アクションを使用して、個々の成果物にカスタムの保持期間を定義することもできます。 詳しい情報については、「[ワークフローデータを成果物として保存する](/actions/guides/storing-workflow-data-as-artifacts#configuring-a-custom-artifact-retention-period)」を参照してください。

## 成果物の有効期限を探す

API を使用して、成果物の削除がスケジュールされている日付を確認できます。 詳しい情報については、「[リポジトリの成果物の一覧表示](/rest/reference/actions#artifacts)」によって返される `expires_at` 値を参照してください。
