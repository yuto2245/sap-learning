# マイグレーションのポイント

S/4HANAへのマイグレーションでは、アセスメント調査で「修正対象」と判定された箇所を中心に改修を進めます。  
基本方針は以下の通りです。

- アセスメントでエラー箇所および影響範囲の調査
- 改修方針の策定（改修パターンごとに標準的な改修案を作成）
- プログラム単位での改修実施（当該プログラムに付随する影響箇所も含めた修正）

システムのバージョン変更に伴うアップグレード対応に加えて、ECC→S/4HANAで頻出する対応内容に対処する必要があります。

## ECCからS/4HANAへのマイグレーションで特に重要なポイント

- **品目番号（MATNR）のフィールド長拡張**  
  ECCでは18桁であった品目番号（MATNR）が、S/4HANAでは**40桁**に拡張されました。  
  業務上40桁を活用しない場合でも、データベースレベルで拡張されるため、カスタムプログラム・インターフェース・レポート・IDoc等でのMATNR関連変数定義は全て見直しが必要です。

- **Universal Journal（ACDOCAテーブル）への統合**  
  従来のFI/CO/AA/MLの個別テーブルが1つのUniversal Journalに統合され、リアルタイム集計が可能となりました。これに伴い、会計関連のカスタムプログラムは大幅な修正が必要です。

- **Unicode対応**
　Unicode化により、文字列操作を伴う命令の中にバイト文字列を前提としたコードが存在する。
　バイト文字列をカウントする際に単純に文字数でカウントされてしまう為、プログラムのずれが発生してしまう。
　そこで、CHARLRN,CONCATENATE,FIND,TRANSLATE,OFFSETの使用箇所などが対象。 

- **Business Partner（BP）への統合**  
  得意先マスタ（KNA1）と仕入れ先マスタ（LFA1）がBusiness Partnerマスタに統合されます。  
  従来の顧客・仕入先番号とのリンク（CVI）設定とデータ移行が必須となります。

- **データモデル簡素化（Simplification）**  
  多くの集計テーブル・索引テーブル（例：BSIS/BSAS、MKPF/MSEGなど）が廃止され、CDSビューやHANAビューへの置き換えが必要です。

- **コード適応（Custom Code Adaptation）**  
  - 廃止機能（例：クラシックバッチインプット、特定のレポート）の置き換え  
  - 権限チェックの変更（AUTHORITY-CHECKの強化）  
  - 外部結合の制限（FOR ALL ENTRIESの代替としてCDSビュー推奨）

- **新機能の活用検討**  
  - MRP Live、Advanced ATP、Embedded Analytics（CDSビュー＋Fiori）  
  - SAP Fiori UIへの移行

---

## 参考リンク

- <a href="https://community.sap.com/t5/enterprise-resource-planning-blogs-by-sap/material-number-field-length-extension-in-sap-s-4hana/ba-p/135xxxx" target="_blank" rel="noopener noreferrer">Material Number Field Length Extension（MATNR 40桁）</a>
- <a href="https://help.sap.com/docs/SAP_S4HANA_ON-PREMISE" target="_blank" rel="noopener noreferrer">S/4HANA Simplification List（主要変更点一覧）</a>
- <a href="https://www.sap.com/products/erp/s4hana.html" target="_blank" rel="noopener noreferrer">SAP S/4HANA 公式製品ページ</a>
