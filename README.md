import { AxiosInstance } from 'axios';
import { ILogger } from '../../../../../../../common/scripts/contracts/ILogger';
import { API_DEFAULT_TIMEOUT, getApiClient } from '../../../scripts/env';
import { ITableEditorService } from '../../../../../../../common/scripts/contracts/ITableEditorService';
import { ITableEditorColumnsInfo, ITableEditorTableNames } from '../../../../../../../common/scripts/models/ITableEditor';

export class AttributeConfigurationService implements ITableEditorService {
  readonly currentServiceName: string = 'AttributeConfigurationService';
  _logger: ILogger;
  _apiClient: AxiosInstance;

  constructor(logger: ILogger, authToken?: any, user?: string) {
    this._logger = logger;
    this._apiClient = getApiClient(
      getMarvelOperationalServicesEnv().MARVEL_APP_BOOK_ROLL_SERVICE_BASE_URL,
      API_DEFAULT_TIMEOUT,
      {},
      authToken,
      user
    );
    this.getTablesNames = this.getTablesNames.bind(this);
    this.getTableSchemaByTableName = this.getTableSchemaByTableName.bind(this);
    this.getTableDataByTableName = this.getTableDataByTableName.bind(this);
  }

  async getTablesNames(): Promise<ITableEditorTableNames[]> {
    let data: Array<ITableEditorTableNames> = [];
    try {
      const response = await this._apiClient.get<Array<ITableEditorTableNames>>('TableEditor/tables');
      data = response?.data;
      return data;
    } catch (err: any) {
      this._logger?.error('MARVEL CONTROLS', this.currentServiceName, err, 'Error getting table names');
      throw err;
    }
  }

  async getTableSchemaByTableName(tableName: string): Promise<ITableEditorColumnsInfo[]> {
    let data: Array<ITableEditorColumnsInfo> = [];
    try {
      const response = await this._apiClient.get<Array<ITableEditorColumnsInfo>>(
        `TableEditor/tableColumnData/${tableName}`
      );
      data = response?.data;
      return data;
    } catch (err: any) {
      this._logger?.error('MARVEL CONTROLS', this.currentServiceName, err, `Error getting table schema for ${tableName}`);
      throw err;
    }
  }

  async getTableDataByTableName(tableName: string): Promise<Record<string, any>[]> {
    let data: Array<Record<string, any>> = [];
    try {
      const response = await this._apiClient.get<Array<Record<string, any>>>(
        `TableEditor/tableData/${tableName}`
      );
      data = response?.data;
      return data;
    } catch (err: any) {
      this._logger?.error('MARVEL CONTROLS', this.currentServiceName, err, `Error getting table data for ${tableName}`);
      throw err;
    }
  }

  // Other existing methods can be added here as needed
}