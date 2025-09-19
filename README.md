fix: Update TableEditorTableNames model to use TableName property

- Changed property Name to TableName in TableEditorTableNames class to match frontend interface expectations
- This resolves the undefined tableName issue when frontend calls getTableSchemaByTableName API