export class AttributeConfigurationService implements ITableEditorService {
  async getTableDataByTableName(tableName: string): Promise<Record<string, any>[]> {
    let data: Array<Record<string, any>> = [];
    try {
      const response = await this._apiClient.getArray<Record<string, any>>(
        `TableEditor/tableColumnData/${tableName}`
      );
      // Strip 'data:' from every row, even if response itself has one
      data = Array.isArray(response) 
        ? response.map(row => row?.data || {}) 
        : Array.isArray(response?.data) 
          ? response.data.map(row => row?.data || {}) 
          : [];
      console.log(data[0]); // should now be raw object
      return data;
    } catch (err: any) {
      this._logger.error(
        'MARVEL OPERATIONAL SERVICE - ' + this.currentServiceName,
        err,
        `Error getting table data for ${tableName}`
      );
      throw err;
    }
  }
}