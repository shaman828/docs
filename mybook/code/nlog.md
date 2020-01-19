## Nlog.config(始终复制)
``` xml
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      throwExceptions="false"
      >
  <variable name="logContent" value="-----------------------${time}-Start-------------------------
${newline}machinename:${machinename}
${newline}message:${message}
${newline}appdomain:${appdomain}
${newline}assembly-version:	${assembly-version}
${newline}processname:${processname}
${newline}exception:${exception:format=tostring}
${newline}stacktrace:${stacktrace}
${newline}-----------------------${time}-End----------------------------" />

  <targets>
    <target name="logtarget" xsi:type="File"
            fileName="${basedir}/logs/${shortdate}.log"
            concurrentWrites="true"
            archiveAboveSize="10485760"
            maxArchiveFiles="20"
            archiveEvery="Hour"
            archiveFileName="${basedir}/logs/archive/${shortdate}-{####}.log"
            archiveNumbering="Rolling"
             layout="${logContent}" />
  </targets>
  <rules>
    <logger name="*" level="Debug,Error,Info" writeTo="logtarget" />
  </rules>
</nlog>
```

- archiveAboveSize="10485760"
    + 10mb自动拆分归档
    + 单位:byte

- archiveEvery="Hour" 是否在每个设定时间刻自动存档日志文件。

    - Day – 每日存档。
    - Hour – 每小时存档。
    - Minute – 每分钟存档。
    - Month – 每月存档。
    - None – 不按时间固定存档。
    - Year – 每年存档。
---
## Logger.cs
``` c#
public class Logger
    {
        NLog.Logger logger;

        private Logger(NLog.Logger logger)
        {
            this.logger = logger;
        }

        public Logger(string name)
            : this(NLog.LogManager.GetLogger(name))
        {
        }

        public static Logger Default { get; private set; }
        static Logger()
        {
            Default = new Logger(NLog.LogManager.GetCurrentClassLogger());
        }

        public void Debug(string msg, params object[] args)
        {
            logger.Debug(msg, args);
        }

        public void Debug(string msg, Exception err)
        {
            logger.Debug(msg, err);
        }

        public void Info(string msg, params object[] args)
        {
            logger.Info(msg, args);
        }

        public void Info(string msg, Exception err)
        {
            logger.Info(msg, err);
        }

        public void Error(string msg, params object[] args)
        {
            logger.Error(msg, args);
        }

        public void Error(string msg, Exception err)
        {
            logger.Error(msg, err);
        }

    }
```
